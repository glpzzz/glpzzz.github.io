---
title: "Integrating Yii3 packages into Wordpress"
date: 2024-03-23
description: "Using Yii3 packages to render and handle forms in a Wordpress website"
image: https://htmx.org/img/memes/bellcurve2.png
---

I recently faced the task to add several long forms to a Wordpress website. Those forms had a lot of fields, sometimes
complex validation rules, even relationships between them where some PHP inheritance could be used to avoid code
duplication, etc.

Quickly noticed that when handling forms in Wordpress the usual thing to do is to install a plugin or manually add the
markup using the editor or in a custom page template. And then you mostly depend on the plugin functionality to handle
the form submission or to code it yourself.

Because part of the task involved logging some data, sending to API endpoints, sending emails or other, I decided to
code the functionality by myself instead of checking if any plugins supported those functionalities.

But also I had read that, at the moment (March
2024), [most Yii 3 packages are ready for production](https://www.yiiframework.com/yii3-progress) and being a long time
user of the framework I decided it was a good opportunity to try and learn about those.

# Goal

Render and handle forms in a Wordpress website using Yii3 packages. For the purpose of the example, the form is going to
be a simple Rating Form and we are just going to send the collected data via email.

# How

Let's start from a minimap classic theme for the sake of the example. We have created a page in Wordpress dashboard with
the name **The Rating Form** and if we want to have a custom template for it, then we create a file
named `page-the-rating-form.php` in the root folder of the theme.

This is the file where we are going to define the markup of our form.

## Add Yii3 packages to the project.

Let's use some Yii3 packages here:

- [yiisoft/form-model](https://github.com/yiisoft/form-model) to define a model class representing the fields of the
  form
- [yiisoft/form](https://github.com/yiisoft/form) to render the form markup
- [yiisoft/validator](https://github.com/yiisoft/validator) to validate the input

In order to install those let's start by creating a Composer project in the root of the theme using `composer init`.
After finishing this process we will endup with a composer.json file.

Let's add the Yii3 packages to the project:

```
composer require yiisoft/form-model:dev-master yiisoft/validator yiisoft/form:dev-master
```

and instruct the theme to load the composer autoload by adding the following line to the `functions.php` file:

```
require ABSPATH . '/vendor/autoload.php';
```

## Create the form model

As a result we also have a `src` directory now on the theme root directory. Let's add our form model class here.
Assuming that the project is going to grow, let's keep things organized and create the directory `src/Forms` and
the `RatingForm` class inside it.

```php
<?php

namespace Glpzzz\Yii3press\Forms;

use Yiisoft\FormModel\FormModel;

class RatingForm extends FormModel
{

	private ?string $name = null;
	private ?string $email = null;
	private ?int $rating = null;
	private ?string $comment = null;
	private string $action = 'the_rating_form';

	public function getPropertyLabels(): array
	{
		return [
			'name' => 'Name',
			'email' => 'Email',
			'rating' => 'Rating',
			'comment' => 'Comment',
		];
	}

}
```

Besides the fields that we need for our rating use case, notice the `action` class attribute. This is special and
important to tell Wordpress which theme hook should handle the form submission. More details on this later.

## Add validation rules to the model

Let's add some validation rules to the model to validate the input. first let's make the class to implement
the `RulesProviderInterface` so the form package can read those and enrich the HTML markup with native validation
attributes.

```php
class RatingForm extends FormModel implements RulesProviderInterface
```

Now we need to implement the `getRules()` method on the class.

```php
public function getRules(): iterable
{
	return [
		'name' => [
			new Required(),
		],
		'email' => [
			new Required(),
			new Email(),
		],
		'rating' => [
			new Required(),
			new Integer(min: 0, max: 5),
		],
		'comment' => [
			new Length(min: 100, skipOnEmpty: true),
		],
	];
}
```

## Create the form markup

To create the form markup we need to pass an instance of `RatingForm` to the template. The way that I found to do this
in Wordpress was to create a global variable (yes, not cool) before rendering the page:

```php

$hydrator = new Hydrator(
	new CompositeTypeCaster(
		new NullTypeCaster(emptyString: true),
		new PhpNativeTypeCaster(),
		new HydratorTypeCaster(),
	)
);

add_filter('template_redirect', function () use ($hydrator) {
	// Get the queried object
	$queried_object = get_queried_object();

	// Check if it's a page
	if ($queried_object instanceof WP_Post && is_page()) {
		if ($queried_object->post_name === 'the-rating-form') {
			global $form;
			if ($form === null) {
				$form = $hydrator->create(RatingForm::class, $_REQUEST['data'] ?? []);
			}

			if (isset($_REQUEST['data'])) {
				(new Yiisoft\Validator\Validator())->validate($form);
			}
		}
	}
});
```

Notice that we also created an instance of the `Hydrator` class outside of any class to be able to reuse it for all the
required callbacks. Now that we have the instance of `RatingForm`, let's create the markup for the form on
the `page-the-rating-form.php`
file.

```html

<?php

use Glpzzz\Yii3press\Forms\RatingForm;
use Yiisoft\FormModel\Field;
use Yiisoft\Html\Html;

/** @var RatingForm $form */
global $form;

?>


<?php get_header(); ?>

<h1><?php the_title(); ?></h1>

<?php the_content(); ?>

<?= Html::form()
	->post(esc_url(admin_url('admin-post.php')))
->open()
?>

<?= Field::hidden($form, 'action')->name('action') ?>
<?= Field::text($form, 'name') ?>
<?= Field::email($form, 'email') ?>
<?= Field::range($form, 'rating') ?>
<?= Field::textarea($form, 'comment') ?>

<?= Html::submitButton('Send') ?>

<?= "</form>" ?>

<?php get_footer(); ?>
```

Here we use the combination of the `Html` helpers and `Field` class from Yii3 to generate the markup of our form.

Notice that:

- The form is going to use the POST method and action was defined as the `admin-post.php` Wordpress endpoint
- We used a hidden field to include the `action` value in the form submission and renamed the input to `action`.

This was done in order to hook into a theme function to handle the form request as is going to be explained in the
following section.

But first, let's use the goodies of Yii to enrich the form. We already defined the validation rules in the model. Those
are good to validate the input once is sent, but what if we want to also validate in the browser? Yes, we could again
define matching those validation rules on the input elements, but here comes Yii to help us with that if we include
the following in the `functions.php` file:

```php
add_action('init', function () {
	ThemeContainer::initialize(
		[
			'default' => [
				'enrichFromValidationRules' => true,
			]
		], 'default', new ValidationRulesEnricher()
	);
});
```

With this code, we are activating the `ValidationRulesEnricher` for the default form theme. When activated, we will see
that the forms fields now include some validation rules (required, min, max) according to the validation rules already
defined on the model class, saving us time and manually written code. Yes, this is very cool Yii3 stuff.

## Process the POST request



# Conclusion

- The used Yii3 packages are ready for production and keep the quality of older Yii framework version.
- It's possible to use those Yii packages in standalone projects, so you can take your Yii knowledge to any PHP project,
  including Wordpress websites.
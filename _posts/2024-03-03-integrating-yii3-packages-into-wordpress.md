---
title: "Integrating Yii3 packages into WordPress"
date: 2024-03-03
description: "Exploring the usage of Yii3 packages to render, validate and handle forms in a WordPress website"
image: https://glpzzz.dev/assets/yii3press.png
---

I was recently assigned with the task of integrating several extensive forms into a WordPress website. These forms
comprised numerous fields, intricate validation rules, dynamic fields (one to many relationships) and even
interdependencies, where employing PHP inheritance could mitigate code duplication.

Upon initial exploration, it became evident that the conventional approach for handling forms in WordPress typically
involves either installing a plugin or manually embedding markup using the editor or custom page templates.
Subsequently, one largely relies on the plugin's functionality to manage form submissions or resorts to custom coding.

Given that part of my task entailed logging data, interfacing with API endpoints, sending emails, and more, I opted to
develop the functionality myself, rather than verifying if existing plugins supported these requirements.

Furthermore, considering the current landscape (as of March 2024) where most Yii 3 packages are deemed production-ready
according to [official sources](https://www.yiiframework.com/yii3-progress), and being a long-time user of the Yii framework, I deemed it an opportune moment to
explore and acquaint myself with these updates.

## Source code available

You can explore the entire project and review the code by accessing it on [Github](https://github.com/glpzzz/yii3press).

Additionally, you can deploy it effortlessly using Docker by simply executing `docker-compose up` from the project's
root directory. Check the `Dockerfile` for the WordPress setup and content generation which is done automatically.

## Goal

My objective was to render and manage forms within a WordPress framework utilizing Yii3 packages. For demonstration
purposes, I chose to implement a basic Rating Form, where the focus is solely on validating the data without executing
further actions.

## Approach

To proceed, let's start with a minimalistic classic theme as an example. I created a WordPress page named "The Rating
Form" within the dashboard. Then, a file named `page-the-rating-form.php` is to be created within the theme's root
folder to display this specific page.

This designated file serves as the blueprint for defining our form's markup.

### Adding Yii3 Packages to the Project:

To harness Yii3's functionalities, we'll incorporate the following packages:

- [yiisoft/form-model](https://github.com/yiisoft/form-model): For defining a model class representing the form's fields
  and facilitating field rendering.
- [yiisoft/form](https://github.com/yiisoft/form): To handle markup-related tasks.
- [yiisoft/validator](https://github.com/yiisoft/validator): For input validation.

To begin, let's initialize a Composer project in the root of our theme by executing `composer init`. This process will
generate a composer.json file. Subsequently, we'll proceed to include the Yii3 packages in our project.

```
composer require yiisoft/form-model:dev-master yiisoft/validator yiisoft/form:dev-master
```

and instruct the theme to load the composer autoload by adding the following line to the `functions.php` file:

```
require __DIR__ . '/vendor/autoload.php';
```

### Create the form model

Following the execution of the `composer init` command, a `src` directory has been created in the root directory of the
theme. We will now proceed to add our form model class within this directory.

Anticipating the expansion of the project, it's imperative to maintain organization. Thus, we shall create the
directory `src/Forms` and place the `RatingForm` class inside it.

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

Beyond the requisite fields for our rating use case, it's crucial to observe the `action` class attribute. This
attribute is significant as it instructs WordPress on which theme hook should manage the form submission. Further
elaboration on this will follow.

### Adding Validation Rules to the Model:

Now, let's incorporate some validation rules into the model to ensure input integrity. Initially, we'll configure the
class to implement the `RulesProviderInterface`. This enables the form package to access these rules and augment the
HTML markup with native validation attributes.

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
			new Length(min: 100),
		],
	];
}
```

### Create the form markup

To generate the form markup, we require an instance of `RatingForm` to be passed to the template. In WordPress, the
approach I've adopted involves creating a global variable (admittedly not the most elegant solution) prior to rendering
the page.

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
				$form = $hydrator->create(RatingForm::class, []);
			}
		}
	}
});
```

It's worth noting that we've instantiated the `Hydrator` class outside any specific function, enabling us to reuse it
for all necessary callbacks. With the `RatingForm` instance now available, we'll proceed to craft the markup for the
form within the `page-the-rating-form.php` file.

```php

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

In the markup generation of our form, we've leveraged a combination of Yii3's `Html` helpers and the `Field` class.
Notable points include:

- The form employs the POST method with the action specified as the `admin-post.php` WordPress endpoint.
- To include the `action` value in the form submission, we utilized a hidden field named `'action'`. We opted to rename
  the input to `'action'` as the `Field::hidden` method generates field names in the
  format `TheFormClassName[the_field_name]`, whereas we required it to be simply named `'action'`.

This adjustment facilitates hooking into a theme function to handle the form request, as elucidated in the subsequent
section.

Before delving further, let's capitalize on Yii's capabilities to enhance the form. Although we've already defined
validation rules in the model for validating input post-submission, it's advantageous to validate input within the
browser as well. While we could reiterate defining these validation rules directly on the input elements, Yii offers a
streamlined approach. By incorporating the following code snippet into the `functions.php` file:

```php
add_action('init', function () {
	ThemeContainer::initialize([
			'default' => [
				'enrichFromValidationRules' => true,
			]
		], 'default', new ValidationRulesEnricher()
	);
});
```

By implementing this code snippet, we activate the `ValidationRulesEnricher` for the default form theme. Upon
activation, we'll notice that the form fields are now enriched with validation rules such as 'required', 'min', and '
max', aligning with the validation rules previously defined in the model class. This feature streamlines the process,
saving us valuable time and minimizing the need for manual code composition. Indeed, this showcases some of the
remarkable functionality offered by Yii3.

### Process the POST request

When the form is submitted, it is directed to `admin-post.php`, an endpoint provided by WordPress. However, when dealing
with multiple forms, distinguishing the processing of each becomes essential. This is where the inclusion of
the `action` value in the POST request proves invaluable.

Take note of the initial two lines in the following code snippet: the naming convention for the hook
is `admin_post_<action_name>`. Therefore, if a form has `action = 'the-rating-form'`, the corresponding hook name will
be `admin_post_the_rating_form`.

As for the inclusion of both `admin_post_<action_name>` and `admin_post_nopriv_<action_name>`, this is because WordPress
allows for different handlers depending on whether the user is logged in or not. In our scenario, we require the same
handler regardless of the user's authentication status.

```php
add_action('admin_post_the_rating_form', fn() => handleForms($hydrator));
add_action('admin_post_nopriv_the_rating_form', fn() => handleForms($hydrator));

function handleForms(Hydrator $hydrator): void
{
  global $form;
  $form = $hydrator->create(RatingForm::class, $_POST['RatingForm']);
  $result = (new Yiisoft\Validator\Validator())->validate($form);

  if ($form->isValid()) {
    // handle the form
  }

  get_template_part('page-the-rating-form');
}
```

Returning to the Yii aspect: we instantiate and load the posted data into the form utilizing the `hydrator`. We then
proceed to validate the data. If the validation passes successfully, we can proceed with the intended actions using the
validated data. However, if validation fails, we re-render the form, populating it with the submitted data and any error
messages generated during validation.

## Conclusion

- This was my first attempt at mixing Yii3 packages with a WordPress site. While I'm satisfied with the result, I think
  it can be improved, especially regarding the use of global variables. Since I'm not very experienced with WordPress,
  **I'd appreciate any suggestions for improvement**.
- The Yii3 packages I used are ready for real-world use and offer the same quality and features as their older versions.
- Now you can use these Yii packages independently. This means you can apply your Yii skills to any PHP project.
- This project shows how we can enhance a WordPress site by tapping into the powerful features of Yii, while still
  keeping the simplicity of the CMS.
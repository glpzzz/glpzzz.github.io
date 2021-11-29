---
title: "How to email error logs to developer on Yii2 apps"
date: 2020-09-15
description: "Use yii\\log\\EmailTarget to receive error messages/warnings from your Yii2 application directly in your inbox."
image: https://glpzzz.is-a.dev/assets/yii2-email-error-logs.png
---
Logging is a very important feature of the application. It let's you know what is happening in every moment. By default, Yii2 basic and advanced application have just a [\yii\log\FileTarget](https://www.yiiframework.com/doc/api/2.0/yii-log-filetarget) target configured.

To receive emails with messages from the app, setup the log component to email (or Telegram, or slack) transport instead (or besides) of file transport:

```php
'components' => [
    // ...
    'log' => [
         'targets' => [
             [
                 'class' => 'yii\log\EmailTarget',
                 'mailer' => 'mailer',
                 'levels' => ['error', 'warning'],
                 'message' => [
                     'from' => ['log@example.com'],
                     'to' => ['developer1@example.com', 'developer2@example.com'],
                     'subject' => 'Log message',
                 ],
             ],
         ],
    ],
    // ...
],
```

The [\yii\log\EmailTarget](https://www.yiiframework.com/doc/api/2.0/yii-log-emailtarget) component is another way to log  messages, in this case emailing them via the `mailer` component of the application as specified on the `mailer` attribute of `EmailTarget` configuration. Note that you can also specify messages properties and which levels of messages should be the sent trough this target.

If you want to receive messages via other platforms besides email, there are other components that represents log targets:
* [Telegram](https://github.com/sergeymakinen/yii2-telegram-log)
* [Slack](https://github.com/sergeymakinen/yii2-slack-log)

Or you can implement your own by subclassing
[\yii\log\Target](https://www.yiiframework.com/doc/api/2.0/yii-log-target)

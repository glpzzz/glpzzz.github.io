---
layout: home
title: "How to redirect all emails to one inbox on Yii2 applications"
date: 2020-10-02
---
[\yii\mail\BaseMailer::useFileTransport](https://www.yiiframework.com/doc/api/2.0/yii-mail-basemailer#$useFileTransport-detail) is a great tool. If you activate it, all
emails sent trough this mailer will be saved (by default) on `@runtime/mail`
instead of being sent, allowing the devs to inspect thre result. 

But what happens if we want to actually receive the emails on our inboxes. When
all emails are suppose to go to one account, there is no problem: setup it as
a param and the modify it in the `params-local.php` (assuming advaced
application template).

The big issue arises when the app is supposed to send emails to different
accounts and make use of replyTo, cc and bcc fields. It's almost impossible try
to solve it with previous approach and without using a lot of `if(YII_DEBUG)`.

Well, next there is a solution: 

```
'useFileTransport' => true,
'fileTransportCallback' => function (\yii\mail\MailerInterface $mailer, \yii\mail\MessageInterface $message) {
    $message->attachContent(json_encode([
            'to' => $message->getTo(),
            'cc' => $message->getCc(),
            'bcc' => $message->getBcc(),
            'replyTo' => $message->getReplyTo(),
        ]), ['fileName' => 'metadata.json', 'contentType' => 'application/json'])
        ->setTo('debug@mydomain.com') // account to receive all the emails
        ->setCc(null)
        ->setBcc(null)
        ->setReplyTo(null);

    $mailer->useFileTransport = false;
    $mailer->send($message);
    $mailer->useFileTransport = true;

    return $mailer->generateMessageFileName();
}

```

How it works? `fileTransportCallback` is the callback to specify the filename that should be used to create the saved email on `@runtime/mail`. It "intercepts" the send email process, so we can use it for our porpuses.

1. Attach a json file with the real recipients information so we can review it
2. Set the recipient (TO) as the email address where we want to receive all the
   emails.
3. Set the others recipients fields as null
4. Deactivate `useFileTransport`
5. Send the email
6. Activate `useFileTransport`
7. Return the defaut file name (datetime of the operation)

This way we both receive all the emails on the specified account and get them stored
on `@runtime/mail`.

Pretty simple helper to review emails on Yii2 applications.

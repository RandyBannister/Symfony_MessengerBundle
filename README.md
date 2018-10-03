Simplify message sending processes in a Symfony2 project.

Imagine that your application sends emails, triggers UI notifications, sends SMS. 
You are using several libraries to do it.
And if you did not factorize your code, you will have bunch of copy paste in every place you are sending something.

Worst, if you was expecting to send an email some day, you may send also an SMS tomorrow. 
If that happen, you will need to rewrite your code...

This bundle is trying to help with all these problems, providing a simple way to configure a distribute these messages.


Principles
----------

### Message

A `message` represent the fact that your application is sending something.

Creating a `message` is as easy as creating a service of class `MessengerBundle\Message` 
with at least 1 `messenger.message` tag, 
for each tag you specify the `channel` on which the message should be distributed.

You can also register it using this bundle [configuration](Resources/docs/configuration.md#messages).

### Recipient

A `recipient` represent the information about the target of your message.

Every `channel` is free to support only certain types of recipient (mostly by checking interfaces).

### Channel

A `channel` represent a way to distribute `messages`.

This bundle come with some built-in `channels`:

- `MessengerBundle\Channel\SwiftmailerChannel` : 
    sending an email with [Swift Mailer](https://github.com/swiftmailer/swiftmailer).
    Read the channel [documentation](Resources/docs/channels/swiftmailer.md)
- `MessengerBundle\Channel\DoctrineChannel` : 
    recording a database entry with [Doctrine ORM](https://github.com/doctrine/doctrine2).
    Read the channel [documentation](Resources/docs/channels/doctrine.md)
- `MessengerBundle\Channel\MobileChannel` : 
    pushing a mobile notification with [NotificationPusher](https://github.com/Ph3nol/NotificationPusher).
    Read the channel [documentation](Resources/docs/channels/mobile.md)

Creating a `channel` is as easy as creating a service that implements `MessengerBundle\Channel\ChannelInterface` 
with the `messenger.channel` tag.

A `channel` will be asked to handle a `delivery` whenever a message is about to be sent.

### Delivery

A `delivery` (`MessengerBundle\Delivery`) represent the `message` about to be sent to a `recipient` for a `channel`.
It is mainly matter of storing all the data in the same place.

Creating a `delivery` is an internal process, that must be done by the `sender`.

## Sender

The `sender` is your entry point for sending messages.

It centralize the configuration of which `messages` to send over which `channels`. 


Installation
------------

### Add the bundle as dependency with Composer

``` bash
$ php composer.phar require /messenger-bundle
```

### Enable the bundle in the kernel

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = [
        // ...
        new MessengerBundle\YokaiMessengerBundle(),
    ];
}
```

### Configuration

Please [read](Resources/docs/configuration.md) the dedicated documentation.


Usage
-----

Please [read](Resources/docs/usage.md) the dedicated documentation.

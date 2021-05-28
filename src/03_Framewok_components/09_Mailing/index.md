# The Mailing Component

This component came from a need to be able to interact with a mailer object in order create emails programmatically more easily. And also the need to be able to swap the default mailer in a project.

For example, we had a case where a client didn't want to send emails from the server, but wanted them to be sent from a third party mailing platform instead.

If you use the wp_mail function everywhere in your code, achieving this scenario could be very difficult.

As always, we've tried to structure this a little, by providing an interface, and some classes implementing it. The mailing component provides therefore a Mailer interface, a WpMailer, which provides a wp_mail implementation, but it also provides a Mandrill and a SwiftMailer implementation.

You can have a look at the [MailerInterface](https://github.com/wonderwp/Mailing/blob/develop/src/MailerInterface.php) to get a sense of the methods provided.

By default, the container holds the [WpMailer](https://github.com/wonderwp/Mailing/blob/develop/src/WpMailer.php) implementation. You can keep that, use one of the other Mailer provided, or write your own.

## Using the mailer

In your manager, register a service that would use the mailer reference from the container.

```
public function register(Container $container)
{
    parent::register($container);
    
    $this->addService('mailHandler', function () use($container) {
        return new ServiceThatNeedsToMailThings($container['wwp.emails.mailer']);
    });       
}
```

This way if you want to change the mailer in the future you'd just have to change the mailer reference again in the container without having to rechange neither your manager nor your service.

Then in you service implementation, use the mailer.

```
$mailer = $this->mailInstance;
$mailer->setSubject($subject);
$mailer->setBody($body);
$mailer->addTo($toMail, $toName);
$mailre->setFrom($fromMail, $fromName);
$sent = $mailer->send($opts);
```

Have a look at the [MailerInterface](https://github.com/wonderwp/Mailing/blob/develop/src/MailerInterface.php) to see a full list of supported mailer methods.

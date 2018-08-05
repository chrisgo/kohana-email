# Email Module For Kohana 3.x and Koseven

Factory-based email class. This class is a simple wrapper around [Swiftmailer](http://github.com/swiftmailer/swiftmailer).

## Updates

* Based on the first email module for Kohana 3.x (shadowhand/email)
* Uses latest version of Swiftmailer (6.x) which removes ::newInstance() methods
* Adds support for using views similar to `View::factory(...)` as email body
* Works with [Koseven](https://github.com/koseven/koseven) and [Cascading filesystem](https://docs.koseven.ga/guide/kohana/files)

## Installation

1. Download to modules directory.
2. Include it in `APPPATH/bootstrap.php` modules list:
```php
Kohana::modules(array(
	...
	'email' => MODPATH.'email',
	...
));
```
3. Go to your `DOCROOT` and include latest Swiftmailer
```
composer require swiftmailer/swiftmailer
```

## Usage

Create new messages using the `Email::factory($subject, $message)` method. Add recipients, add sender, send message:

```
$email = Email::factory('Hello, World', 'This is my body, it is nice.')
              ->to('person@example.com')
              ->from('you@example.com', 'My Name')
              ->send();
```

You can also add HTML to your message:

```
$email->message('<p>This is <em>my</em> body, it is <strong>nice</strong>.</p>', 'text/html');
```

You can also use a view and bind variables (like in the controllers)

```
$email = Email::factory('Hello, World', 'This is my body, it is nice.')
              ->to('person@example.com')
              ->from('you@example.com', 'My Name')
              ->view('emails/notify')
              ->bind('email', $email)
              ->send();
```

Additional recipients can be added using the `to()`, `cc()`, and `bcc()` methods.

Additional senders can be added using the `from()` and `reply_to()` methods. If multiple sender addresses are specified, you need to set the actual sender of the message using the `sender()` method. Set the bounce recipient by using the `return_path()` method.

To access and modify the [Swiftmailer message](http://swiftmailer.org/docs/messages) directly, use the `raw_message()` method.

## Configuration

Configuration is stored in `config/email.php`. Options are dependant upon transport method used. Consult the Swiftmailer documentation for options available to each transport.

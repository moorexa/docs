# Aliasing
Class | Method
------|-------
Lightroom\Core\BootCoreEngine|registerAliases()

The alias method available in ```src/config/aliases.php``` would help you register a class, trait, or interface with or without a namespace. 

It instructs the autoloader to include a registered PHP file when its class, interface, trait is called globally.

This is a basic structure.

```php
// Application Aliases
/** @var mixed $config **/
$config->alias([
    '<class|interface|trait>' => '<file path>',
]);
```

Lets look at practical solution. Imagine you have a file called ```mail_class.php``` saved in a folder named ```messaging```. 

```php
// inside messaging/mail_class.php 
namespace Messaging;

// class definition
class Mail
{
    // send message
    public static function send()
    {
        echo 'sent!';
    }
}
```

We can both agree that one way to use this file is to include it on every PHP file it's required, or have it included through an autoloader (see [https://github.com/moorexa/docs/tree/master/autoloading]).

But we would run into an error when trying to call the ```Messaging\Mail``` class using the default autoloader. Why?
1. The folder was named ```messaging``` instead of ```Messaging```
2. The file was named ```mail_class.php``` instead of ```Mail.php```

To overcome this limitation, we utilize the alias manager in registering the ```Messaging\Mail``` class for a general usage from within our application. 

## How to register
```php
// Application Aliases
/** @var mixed $config **/
$config->alias([
    Messaging\Mail::class => 'messaging/mail_class.php',
]);
```
Moorexa would include or import the registered ```messaging/mail_class.php``` file when ```Messaging\Mail``` class is called.

Now we can use the ```Messaging\Mail``` class anywhere in our application. See the basic usage below.

```php
use Messaging\Mail;

// call send function
Mail::send(); // => 'sent!'
```

### Help us make this doc great!

All Moorexa docs are open source. See something that's wrong or unclear? Submit a pull request.

Thanks.
# Aliasing
File ```src/config/aliases.php```

```php
// Application Aliases
/** @var mixed $config **/
$config->aliase([
    '<class|interface|trait>' => '<file path>',
]);
```

Aliasing would help you register a class, trait, or interface with or without a namespace, and include it only when you request for it. 

Lets look at practical solution. Imagine you created a file called ```mail_class.php``` saved in a folder named ```messaging```. 

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

We can both agree that one way to use this file is to include it on every file it's required or use an autoloader [see [https://github.com/moorexa/docs/tree/master/autoloading]].

But we would run into an error when trying to call the MAIL class using our autoloader, because of the following.
1. The folder was named ```messaging``` instead of ```Messaging```
2. The file was named ```mail_class.php``` instead of ```Mail.php```

To overcome this limitation, we utilize the alias manager to register our ```Messaging\Mail``` class without worries and for a general usage anywhere in our application. 

See application below;

```php
// Application Aliases
/** @var mixed $config **/
$config->aliase([
    Messaging\Mail::class => 'messaging/mail_class.php',
]);
```
Moorexa would handle the file inclusion only when it is required, keeping your application fast and light.

Now we can use the ```Messaging\Mail``` class anywhere in our application. See example below.

```php
use Messaging\Mail;

// call send function
Mail::send(); // => 'sent!'
```

### Help us make this doc great!

All Moorexa docs are open source. See something that's wrong or unclear? Submit a pull request.

Thanks.
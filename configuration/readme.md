# Configuration
Class | Method
------|--------
Lightroom\Packager\Moorexa\BootloaderConfiguration | loadBootstrap
Lightroom\Packager\Moorexa\BootloaderConfiguration | loadFinder

You may need to start with the ```src/config/config.php``` file when the need for configuration arises. The ```config.php``` file abstracts 2 methods for your convinence, all thanks to the ```BootloaderConfiguration``` class provided by the moorexa packager bundled with your current installation. This methods would be discussed in the preceeding headers, but they are?
1. Bootstrap
2. Finder

## Bootstrap
The bootstrap method provides general configuration for your app. It majors in security tokens, url settings, sanitization, input filtering, encryption, and much more. 

Thou, through the ```get_env()``` function we can create, update both custom or default configurations at run time. But before we dive into that, lets examine the bootstrap array and discuss briefly.

Config Name | Value | Description
------------|-------|-------------
app.url | (string) e.g http://example.com | Sets the default URL for our application. (Optional)
coming-soon | (bool) true or false | Would display a coming soon page for all visiting users or deactivate if turned on
default.env | (string) | Holds the default environment yaml file path.
maintaince-mode | (string) true or false | Would display a maintaince page for all visiting users or deactivate if turned on
timezone | (string) | Sets the default timezone for our application
enable.db.caching | (bool) true or false | If set to ```true``` caching for db would be allowed, such that update, insert, delete queries would be saved and ran during migration
force.https | (bool) true or false | This would force https as the default request scheme.
assist_token | (string) | Holds the assist CLI token for production transactions. like DB migration etc.
controller.basepath | (string) | Holds the default base path for all controllers.
controller.namespace_prefix | (string) | Adds an extra prefix to the default namespace for controllers
csrf_salt | (string) | Holds a unique salt for your CSRF (Cross Site Request Forgery) token. Ensure to keep this key private
secret_key | (string) | Holds a unique key for OPENSSL encryption. Ensure to keep this key private
encryption.method | (string) | Holds the default OPENSSL encryption method. e.g AES-256-CBC
secret_key_salt | (string) | Holds a unique secret salt for OPENSSL encryption. 
webasapi_token | (string) | Holds a token for API request handling by the launcher library
sanitize_html | (bool) true or false | When set to true, moorexa would provide a safer output against injections
filter-input | (bool) true or false | When set to true, moorexa would filter, sanitize, and clean all user inputs.
static_url | (string) | Holds a cookie free static url for serving static files. eg static.example.com
http_access_control | (array) | Contains a list of incoming http request headers to accept in your application
controller_config | (array) | Contains a list of configuration for all your controllers. e.g default model, default provider etc.
debug_mode | (bool) true or false | When set to true, moorexa would print all errors to the screen using the registered error handler. But when set to false, moorexa would log all errors using monolog or any registered logger.

That's about all. Now we can show you how to read a configuration setting using the ```env()``` function

```php
// @var string $basePath
$basePath = get_env('bootstrap', 'controller.basepath');

// now we can check
if (file_exists($basePath)) echo 'it works!'; // => it works
```

One more thing my friend. How about we show you how to update a configuration setting?

```php
// @var string $basePath
$basePath = get_env('bootstrap', 'controller.basepath');

// print base path before.
echo $basePath; // => ./app

// change base path
get_env('bootstrap/controller.basepath', './somewhere/safe');

// print base path again
echo get_env('bootstrap', 'controller.basepath'); // => ./somewhere/safe
```

## Finder
The finder method provides an extra layer for namespace management and directory autoloading. You know it's possible for you to have a folder that contains PHP script needed for your application. And its also possible that you would need a quick way to use this files when needed. Well, that's what we would discuss in the preceeding headings below.

### Autoloading files 

The goal here is to help you register nested folders that acts as a namespace to a file. Lets take one example for clearity.

Imagine we have a folder structure like this;
```plain
 - Libaries/
 ----- Validation/
 -------- Validate.php 
```
And our ```Validate.php``` file with a namespace of ```Validation```;

```php
namespace Validation;

// validate class
class Validate
{
    //.. code here
}
```

Now, we register the ```Libaries``` directory and in turn we can call the ```Validation\Validate``` class when needed anywhere in our application.

```php
$config->finder([

	'autoloader' => [
        // register the libaries directory
        HOME . 'Libaries/*' // the asteric is required to check through all files and directories.
    ],
    
    // .. other codes
);
```
 
### Namespacing
The goal here is to help the PHP ```spl_autoload``` function watch for a pattern in a namespace and load from a registered directory if the file exists.

Using this folder structure;
```plain
 - Libaries/
 ----- Validation/
 -------- Validate.php 
```
To demostrate, we can build on our last example. But this time, make the namespace different.

```php
namespace Validation\Basic\Example;

// validate class
class Validate
{
    //.. code here
    public function isString($value) : bool
    {
        // @var bool $matched
        $matched = false;

        // check if it's a string
        if (is_string($value)) $matched = true;
        
        // retrun bool
        return $matched;
    }
}
```

With a namespace like ```Validation\Basic\Example``` it's almost impossible to load the ```Validate``` class. But we possibly can by registering the namespace with an asteric to match for the ```Validate``` class.  

```php
$config->finder([
    'namespacing' => [
		'Validation\Basic\Example\*'	=> HOME . '/Libaries/',
	]
]);
```

Now, we can successfully use the ```Validate``` class anywhere in our application. 

```php 
// create instance.
$validate = new Validation\Basic\Example\Validate();

// check if this is a string
if ($validate->isString('Moorexa')) echo 'so cool'; // => so cool
```

### Help us make this doc great!

All Moorexa docs are open source. See something that's wrong or unclear? Submit a pull request.

Thanks.
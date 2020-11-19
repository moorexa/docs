# Controller paths
Class | Method
------|-------
Lightroom\Packager\Moorexa\MVC\Helpers\ControllerLoader | setBasePath()

All controllers generated through the assist manager by default gets saved in the ```app/``` root directory. 

In most cases, you may need to incorporate other frameworks with Moorexa, set a base directory for a controller outside the ```app/``` directory or within, during runtime. This is where the ```Lightroom\Packager\Moorexa\MVC\Helpers\ControllerLoader::setBasePath()``` method can be fully utilized.

With the ```src/extra/controllers.php``` file, you can alter the base directory  a controller using the ```setBasePath()``` static method.

The ```src/extra/controllers.php``` file gets called just before the ```Lightroom\Packager\Moorexa\MVC\Helpers\ControllerLoader::serveController()``` method finds the requesting controller class.

### Take this illustration.
Imagine we have a route request like this
```http
GET http://www.example.com/app/home
```

Here is the request breakdown;

method | scheme | host | controller | view
-------|--------|------|------------|-----
GET    | http   | www.example.com   | app  | home

And also imagine we have a folder called ```external/``` that contains a controller class called ```App```. We can channel requests for ```app``` controller to the directory ```external/```.

Lets look into the presumed ```external/``` directory.
```txt
external /
    - main.php (class App{})
    - Models/
    - Providers/
    - ...others
```

Now, we can open up the file ```src/extra/controllers.php```, alter our ```app``` controller base directory like this;

```php
use Lightroom\Packager\Moorexa\MVC\Helpers\ControllerLoader;
// or use 'self'

// alter 'app' base directory
ControllerLoader::setBasePath('app', 'external/');
```

## Extra (How to set the base directory for your controllers)
Navigate to the ```src/config/config.php``` file and open it up. Find ```'controller.base.path'``` and change the default path to your preferred path as demonstrated below;

```php
/*
    ***************************
    * 
    * @config.controller base.path (default = PATH_TO_WEB_PLATFORM) 
    * 
*/
'controller.base.path' => '<your directory path>',
```

You can also use the ```env()``` function to change the base path during runtime as demonstrated below;

```php 
env('bootstrap/controller.base.path', '<your directory path>');
```

### Help us make this doc great!

All Moorexa docs are open source. See something that's wrong or unclear? Submit a pull request.

Thanks.
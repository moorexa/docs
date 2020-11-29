# Containers
Class | Method
------|-------
Lightroom\Adapter\Container | register()

Containers are amazing. You can wrap a class, trait, function, interface with it and then access them via the ```app()``` global function or with a reference.

## Registering a container
1. Using the ```Container::register()``` method. Navigate to the ```src/services/container.php``` file and open it with your favorite IDE e.g PHPSTORM, VSCODE
```php
use Lightroom\Adapter\Container;

// ..code
Container::register([
   /**
    * '<reference>' => <class>
    * 
    * Let's try this example
    * 'mysql' => Lightroom\Database\Drivers\Mysql\Driver::class 
    * 
    * Now, you can access this class via app('mysql'). There are several possibilities here,
    * Please see Lightroom\Adapter\Container for available methods
    */

   // add template handler 
   'screen' => Lightroom\Templates\TemplateHandler::class,
   // where 'screen' is a reference
   // we can access this call via app('screen') or new screen();
   
])
```

2. Using the ```app()``` global function. This can be done anywhere in our application.
```php
// we use template handler for this example
use Lightroom\Templates\TemplateHandler;

// wrap the template handler with a reference
app()->add('template-handler', TemplateHandler::class);
// so we later access this class via
// app('template-handler')->someMethod();
// or
app()->add('Template\Handler', TemplateHandler::class);
// so we can later access this class via
// new Template\Handler();

// add interface
app()->add('controllerInterface', \Lightroom\Packager\Moorexa\Interfaces\ControllerInterface::class);

// add trait
app()->add('AccountTrait', \Lightroom\Requests\Get::class);

// add closure function
app()->add('function', function(string $name){
    return 'hello ' . $name;
});

// add anonymous function
app()->add('myClass', new class(){
    public function sayHello(string $name)
    {
        echo app('function')->invoke($name);
    }
})
```

## Methods available
Here is a complete list of methods available for your consumption when working with the ```Lightroom\Adapter\Container``` class.

method | parameters | description
-------|------------|------------
add()  | ```<reference>, <class,interface,trait,function>``` | Creates a reference to a class, interface, trait, or closure function.
all()  | void | Would return a list of references from the container registry array.
drop() | ```<reference>``` | Would remove a reference from the registry array.
invoke() | ```<arguments>``` | Would invoke a reference closure function.
instance() | ```<arguments>``` | Would return a singleton instance of a reference class.
fresh() | ```<reference>``` | Would create a fresh instance of a reference class.
get() | ```<reference>, <property>``` | Would attempt to get a property from a reference class
set() | ```<reference>, <property>, <value>``` | Would attempt to set a value for a property in a reference class
call() | ```<reference>, <method name>, <arguments>``` | Would attempt to call a method from a reference class with 1 or more arguments



## Accessing a container
1. Using the ```app()``` global function. 

This function provides an extra layer when working with containers. In most cases, you may call the ```app()``` function without any parameter, which would signal that you need to use the ```Lightroom\Adapter\Container``` class to access some of the methods listed above. We would have this clear to you in the code demostration below, lets see some examples shall we?

```php
// wrap mysql query with a reference
app()->add('mysql', Lightroom\Database\Drivers\Mysql::class);

// call a method from a class
$query = app()->call('mysql', 'setTable', 'moorexa_example'); // (static or non static method)

// or access the method directly
$query = app('mysql')->setTable('moorexa_example'); // (static or non static method)

// get a property
$source = app()->get('mysql', 'source'); // (static or non static property)

// or access the property directly
$source = app('mysql')->source; // (static or non static property)

// update a property
app()->set('mysql', 'source', 'default'); // (static or non static property)

// or set the property directly
app('mysql')->source = 'default'; // (static or non static property)

// get mysql instance with 1 or more arguments
$mysql = app('mysql')->instance('args1', 'args2');

// drop mysql reference
app()->drop('mysql');

```
2. Using a reference from any of our examples above.

You can implement a reference as an interface, use a reference as a trait, instantiate a reference as a class. Lets show you how.
```php 
// lets implement the 'controllerInterface' we registered above.
class basicClass implements controllerInterface {
    // .. code here
}

// lets use the trait 'AccountTrait' we registered above.
class anotherClass
{
    use AccountTrait;
}

// lets access the 'Template\Handler' class registered above.
$handler = new Template\Handler(); // now a reference to (Lightroom\Templates\TemplateHandler)
```

### Help us make this doc great!

All Moorexa docs are open source. See something that's wrong or unclear? Submit a pull request.

Thanks.
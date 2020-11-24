# Constants
Class | Property
------|--------
Lightroom\Core\GlobalConstants | $set

Whenever you need to create a constant that applies globally navigate and open up the ```src/config/constants.php``` file with any of your favorite IDE. 

Behind the scene, constants are created with the ```define()``` function so you never have to worry. Remember, constants should be in caps, should be descriptive, clear and meaniful.

To demostrate, we would show you how to apply a constant globally.

```php
// file src/config/constants.php

// ... codes

// add a constant
$set->CONSTANT_NAME = 'CONSTANT_VALUE'; // (mixed data type)
```

Now we can access our newly created constant like this;

```php

// option 1
echo constant('CONSTANT_NAME');

// option 2 
echo CONSTANT_NAME;

```

### Help us make this doc great!

All Moorexa docs are open source. See something that's wrong or unclear? Submit a pull request.

Thanks.
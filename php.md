# PHP

#### Constructure

The php constructs like this:

```php
<?php
content
?>
```

It will appear in the file that the extend name is .php,but note that php can not exist in html, however the .php file has the same constructor and content as html beside it can also be a container of php, like the jsp in J2ee.

## variable

In php, the variable starts with '$', si=o it's format is like this:

```php
$x= 'Hello World!';
$y=24;
$a=3.14;
```

#### Global scope

When we set a variable outside of functions, we call it a global variable, not like the other programming languages, at this place the global variable cannot be invoked in a function directly. We could invoke them by passing parmeters to the function or we could use "global" array that contains all the global variables, and it's index is the names of the global variables, so we could use "global[x]" to invoke the variable "$x" above.

#### Local scope

We could set a global variable, so we could also set a local variable, and like the other programming languages, the local variable can only be used inside a function which one includes it.

#### Static keyword

In php, if we set a local variable, and then we edit it after we invoked this variable, the next time we invoke this function again, we will found that this variable will be reset again,like this:

```php
function Add() {
        $a=1;
        echo $a,"<br/>";
        $a++;
    }
    Add();
    Add();
```

We will always get the same result of 1, but after we asign this $a to a static one, the result will become 1 and 2 .

#### echo and print

In php, there are two different ways to output the data to the screen:
the echo and print. Their deference are that print has a return value of 1, so it can be used in expressions and echo can take not only one parameter(rare use). And the html makeup is permited in both of two methods

```php

```

#### PHP array

In php, we do not need use new to create a new array, it means we will create a array like this:

```php
$cars = array("Volvo","BMW","Toyota");
```

#### PHP Object

In php we use class as a template of an object and take the object as an instance of a class. And the construct function will have the same importance as in Java. But but before we build a constructor, there should be two underline before, and when we set or invoke the properties and methods of the object, we use -> instead of dot.

### String's methods

#### strlen(String):

We use it to get the length of the string

#### str_word_count(String)

We use it to get the counts of the number of words in string.

#### strrev(String)

We use it to reverse the string

#### strpos(current String,target String)

We use it to get the start index of the of the specific String in current String

#### str_replace(target String,substitute String,current String)

We will use the substitute string to replace the target String in current String

#####

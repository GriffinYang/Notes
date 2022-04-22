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

### PHP Numbers

#### Integer

An integer data type is a non-decimal number between -2147483648 and 2147483647 in 32 bit systems, and between -9223372036854775808 and 9223372036854775807 in 64 bit systems.<br>
And there are three constants in integer:<br>
1.PHP_INT_MAX - The largest integer supported<br>
2.PHP_INT_MIN - The smallest integer supported<br>
3.PHP_INT_SIZE - The size of an integer in bytes<br>

#### PHP has the following functions to check if the type of a variable is integer:

is_int(int)<br>
is_integer(int) - alias of is_int()<br>
is_long(int) - alias of is_int()<br>

#### PHP has the following functions to check if the type of a variable

is float:<br>
is_float()<br>
is_double() - alias of is_float()<br>

#### PHP Infinity

A numeric value that is larger than PHP_FLOAT_MAX is considered infinite.<br>
PHP has the following functions to check if a numeric value is finite or infinite:<br>
is_finite()<br>
is_infinite()<br>

##### Note:var_dump() function returns the data type and value:

#### PHP NaN

NaN stands for Not a Number.
NaN is used for impossible mathematical operations.PHP has the following functions to check if a value is not a number:
is_nan()

#### PHP Numerical Strings

The PHP is_numeric() function can be used to find whether a variable is numeric. The function returns true if the variable is a number or a numeric string, false otherwise.

##### Note: From PHP 7.0: The is_numeric() function will return FALSE for numeric strings in hexadecimal form (e.g. 0xf4c3b00c), as they are no longer considered as numeric strings.

#### PHP Math

1.The pi() function returns the value of PI<br>
2.The min() and max() functions can be used to find the lowest or highest value in a list of arguments.<br>
3.The abs() function returns the absolute (positive) value of a number<br>
4.The sqrt() function returns the square root of a number<br>
5.The round() function rounds a floating-point number to its nearest integer:<br>

```php
<?php
echo(round(0.60));  // returns 1
echo(round(0.49));  // returns 0
?>
```

6.The rand([from,to]) function generates a random number

#### PHP Constants

A constant is an identifier (name) for a simple value. The value cannot be changed during the script.A valid constant name starts with a letter or underscore (no $ sign before the constant name).<br>
To create a constant, use the define() function:

```php
//define(name, value, case-insensitive)
define("BESTFRI","TAO XUETING",false);
```

#### In PHP7, you can create an Array constant using the define() function:

```php
<?php
define("cars", [
  "Alfa Romeo",
  "BMW",
  "Toyota"
]);
?>
```

#### PHP divides the operators in the following groups:

1.Arithmetic operators<br>
2.Assignment operators<br>
3.Comparison operators<br>
4.Increment/Decrement operators<br>
5.Logical operators<br>
6.String operators<br>
7.Array operators<br>
8.Conditional assignment operators<br>

#### PHP Arithmetic Operators

<table>
<th>Operator</th>
<th>Name</th>
<th>Example</th>
<th>Result</th>
<tr>
<td>+</td>
<td>Addition</td>
<td>$x+$y</td>
<td>Sum of both values</td>
</tr>
<tr>
<td>-</td>
<td>Substraction</td>
<td>$x-$y</td>
<td>Difference of both values</td>
</tr>
<tr>
<td>*</td>
<td>Multiplication</td>
<td>$x*$y</td>
<td>Product of both values</td>
</tr>
<tr>
<td>/</td>
<td>Division</td>
<td>$x/$y</td>
<td>Qutient of both values</td>
</tr>
<tr>
<td>%</td>
<td>Modulus</td>
<td>$x%$y</td>
<td>Remainder of $x devided by $y</td>
</tr>
<tr>
<td>**</td>
<td>Exponentition</td>
<td>$x**$y</td>
<td>Result of raising $x to the $y'th power</td>
</tr>
</table>

#### PHP Assignment Operators

The PHP assignment operators are used with numeric values to write a value to a variable.The basic assignment operator in PHP is "=". It means that the left operand gets set to the value of the assignment expression on the right.

<table>
<th>Assignment</th>
<th>Same as</th>
<th>Description</th>
<tr>
<td>x=y</td>
<td>x=y</td>
<td>The left operand gets set to the value of the expression on the right</td>
</tr>
<tr>
<td>x+=y</td>
<td>x=x+y</td>
<td>Addition</td>
</tr>
<tr>
<td>x-=y</td>
<td>x=x-y</td>
<td>Substraction</td>
</tr>
<tr>
<td>x*=y</td>
<td>x=x*y</td>
<td>Multiplication</td>
</tr>
<tr>
<td>x/=y</td>
<td>x=x/y</td>
<td>Division</td>
</tr>
<tr>
<td>x%=y</td>
<td>x=x%y</td>
<td>Modulus</td>
</tr>
</table>

#### PHP Comparison Operators

The PHP comparison operators are used to compare two values (number or string):

<table>
<th>Operator</th>
<th>Name</th>
<th>Example</th>
<th>Result</th>
<tr>
<td>==</td>
<td>Equal</td>
<td>$X==$y</td>
<td>Return true if $x is equal to $y</td>
</tr>
<tr>
<td>===</td>
<td>Identical</td>
<td>$x==$y</td>
<td>Return true if $x is equal to $y, and they are the samle type</td>
</tr>
<tr>
<td>!=</td>
<td>Not equal</td>
<td>$x!=$y</td>
<td>Returns true is $x in not equal to $y</td>
</tr>
<tr>
<td><></td>
<td>Not Equal</td>
<td>$x<>$y</td>
<td>Returns true is $x in not equal to $y</td>
</tr>
<tr>
<td>!==</td>
<td>Not Identical</td>
<td>$x!==$y</td>
<td>Returns true is $x in not equal to $y, or they are not the same type</td>
</tr>
<tr>
<td>></td>
<td>Greater Than</td>
<td>$x>$y</td>
<td>Returns true if $x is greater than $y</td>
</tr>
<tr>
<td><</td>
<td>Less Than</td>
<td>$x<$y</td>
<td>Returns true if $x is less than $y</td>
</tr>
<tr>
<td>>=</td>
<td>Greater than or equal to </td>
<td>$x>=$y</td>
<td>Returns true if $x is greater than or equal to $y</td>
</tr>
<tr>
<td><=</td>
<td>Less Than</td>
<td>$x<=$y</td>
<td>Returns true if $x is less than or equal to $y</td>
</tr>
<tr>
<td><=></td>
<td>Spaceship</td>
<td>$x<=>$y</td>
<td>Returns an integer less than, equal to, or greater than zero, depending on if $x is less than, equal to, or greater than $y. Introduced in PHP 7.	
</td>
</tr>
</table>

#### Spaceship example

```php
<?php
$x = 5;
$y = 10;

echo ($x <=> $y); // returns -1 because $x is less than $y
echo "<br>";

$x = 10;
$y = 10;

echo ($x <=> $y); // returns 0 because values are equal
echo "<br>";

$x = 15;
$y = 10;

echo ($x <=> $y); // returns +1 because $x is greater than $y
?>
```

#### HP Increment / Decrement Operators

<table>
<th>Operator</th>
<th>Name</th>
<th>Description</th>
<tr>
<td>++$x</td>
<td>Pre-increment</td>
<td>Increments $x by one, then returns $x</td>
</tr>
<tr>
<td>$x++</td>
<td>Post-increment</td>
<td>Returns $x, then increments $x by one</td>
</tr>
<tr>
<td>--$x</td>
<td>Pre-decrement</td>
<td>Decrements $x by one, then returns $x</td>
</tr>
<tr>
<td>$x--</td>
<td>Psot-decrement</td>
<td>Returns $x, then decrements $x by one</td>
</tr>
</table>

#### PHP Logical Operators

The PHP logical operators are used to combine conditional statements.

<table>
<th>Operator</th>
<th>Name</th>
<th>Example</th>
<th>Result</th>
<tr>
<td>and</td>
<td>And</td>
<td>$x and $y</td>
<td>True if both conditions are true</td>
</tr>
<tr>
<td>or</td>
<td>Or</td>
<td>$x or $y</td>
<td>True if either $x or $y is true</td>
</tr>
<tr>
<td>xor</td>
<td>Xor</td>
<td>$x xor $y</td>
<td>True if either $x or $y is true, but not both</td>
</tr>
<tr>
<td>&&</td>
<td>And</td>
<td>$x && $y</td>
<td>True if both $x and $y are true</td>
</tr>
<tr>
<td>||</td>
<td>Or</td>
<td>$x||$y</td>
<td>True if either $x or $y is true</td>
</tr>
<tr>
<td>!</td>
<td>Not</td>
<td>!$x</td>
<td>True if $x is not true</td>
</tr>
</table>

#### PHP String Operators

PHP has two operators that are specially designed for strings.

<table>
<th>Operator</th>
<th>Name</th>
<th>Example</th>
<th>Result</th>
<tr>
<td>.</td>
<td>Concatenation</td>
<td>$txt1 . $txt2</td>
<td>Concatenation of $txt1 and $txt2</td>
</tr>
<tr>
<td>.=</td>
<td>Concatenation assignment</td>
<td>$txt1 .= $txt2</td>
<td>Appends $txt2 to $txt1</td>
</tr>
</table>

#### PHP Array Operators

The PHP array operators are used to compare arrays.

<table>
<th>Operator</th>
<th>Name</th>
<th>Example</th>
<th>Result</th>
<tr>
<td>+</td>
<td>Union</td>
<td>$x+$y</td>
<td>Union of $x and $y</td>
</tr>
<tr>
<td>==</td>
<td>Equality</td>
<td>$x==$y</td>
<td>Returns true if $x and $y have the same key/value pairs</td>
</tr>
<tr>
<td>===</td>
<td>Identity</td>
<td>$x===$y</td>
<td>Returns true if $x and $y have the same key/value pairs in the same order and of the same types</td>
</tr>
<tr>
<td>!=</td>
<td>Inequality</td>
<td>$x!=$y</td>
<td>Returns true if $x is not equal to $y</td>
</tr>
<tr>
<td><></td>
<td>Inequality</td>
<td>$x!=$y</td>
<td>Returns true if $x is not equal to $y</td>
</tr>
<tr>
<td>!==</td>
<td>Non-identity</td>
<td>$x !== $y</td>
<td>Returns true if $x is not identical to $y</td>
</tr>
</table>

#### PHP Conditional Assignment Operators

The PHP conditional assignment operators are used to set a value depending on conditions:

<table>
<th>Operator</th>
<th>Name</th>
<th>Example</th>
<th>Result</th>
<tr>
<td>?:</td>
<td>Tenary</td>
<td>$x = expr1 ? expr2 : expr3</td>
<td>Returns the value of $x.
The value of $x is expr2 if expr1 = TRUE.
The value of $x is expr3 if expr1 = FALSE</td>
</tr>
<tr>
<td>??</td>
<td>Null coalescing</td>
<td>$x = expr1 ?? expr2</td>
<td>Returns the value of $x.
The value of $x is expr1 if expr1 exists, and is not NULL.
If expr1 does not exist, or is NULL, the value of $x is expr2.
Introduced in PHP 7</td>
</tr>
</table>

### IF ElSE in PHP

Very often when you write code, you want to perform different actions for different conditions. You can use conditional statements in your code to do this.<br/>

#### In PHP we have the following conditional statements:<br/>

1.if statement - executes some code if one condition is true<br/>
2.if...else statement - executes some code if a condition is true and another code if 3.that condition is false<br/>
3.if...elseif...else statement - executes different codes for more than two conditions<br/>
4.switch statement - selects one of many blocks of code to be executed<br/>

### SWITCH in PHP

#### The switch statement is used to perform different actions based on different conditions.

```php
switch (n) {
  case label1:
    code to be executed if n=label1;
    break;
  case label2:
    code to be executed if n=label2;
    break;
  case label3:
    code to be executed if n=label3;
    break;
    ...
  default:
    code to be executed if n is different from all labels;
}
```

#### Loops in PHP

In PHP, we have the following loop types:<br>
1.while - loops through a block of code as long as the specified condition is true<br>

```php
while (condition is true) {
  code to be executed;
}
```

2.do...while - loops through a block of code once, and then repeats the loop as long as the specified condition is true<br>

```php
do {
  code to be executed;
} while (condition is true);
```

3.for - loops through a block of code a specified number of times<br>

```php
for (init counter; test counter; increment counter) {
  code to be executed for each iteration;
}
```

4.foreach - loops through a block of code for each element in an array<br>

```php
foreach ($array as $value) {
  code to be executed;
}
```

#### PHP is a Loosely Typed Language

<br>
PHP automatically associates a data type to the variable, depending on its value. Since the data types are not set in a strict sense, you can do things like adding a string to an integer without causing an error.<br><br>
In PHP 7, type declarations were added. This gives us an option to specify the expected data type when declaring a function, and by adding the strict declaration, it will throw a "Fatal Error" if the data type mismatches.<br><br>
In the following example we try to send both a number and a string to the function without using strict:

```php
//Do not add strict declaration
<?php
function addNumbers($a,$b) {
  return $a + $b;
}
echo addNumbers(5, "5 days");
// since strict is NOT enabled "5 days" is changed to int(5), and it will return 10
?>
```

```php
//Set the type, but not use the strict declaration
<?php
function addNumbers(int $a, int $b) {
  return $a + $b;
}
echo addNumbers(5, "5 days");
/* Cast a fatal error, but we can still add a String like "5" to the parameter, it will run correctly*/
?>
```

```php
//Add strict declaration
<?php declare(strict_types=1);/*declare() must be the very first line of the PHP file including html code*/
function addNumbers(int $a,int $b) {
  return $a + $b;
}
echo addNumbers(5, 5);
// We have to give all parameters with matched type
?>
```

#### PHP Return Type Declarations

PHP 7 also supports Type Declarations for the return statement. Like with the type declaration for function arguments, by enabling the strict requirement, it will throw a "Fatal Error" on a type mismatch.<br>
To declare a type for the function return, add a colon ( : ) and the type right before the opening curly ( { )bracket when declaring the function.<br>
In the following example we specify the return type for the function:<br>

```php
<?php declare(strict_types=1); // strict requirement
function addNumbers(float $a, float $b) : float {
  return $a + $b;
}
echo addNumbers(1.2, 5.2);
?>
```

#### Passing Arguments by Reference

In PHP, arguments are usually passed by value, which means that a copy of the value is used in the function and the variable that was passed into the function cannot be changed.

When a function argument is passed by reference, changes to the argument also change the variable that was passed in. To turn a function argument into a reference, the & operator is used:

```php
<?php
function add_five(&$value) {
  $value += 5;
}

$num = 2;
add_five($num);
echo $num;
?>
```

## Array

#### Create an Array in PHP

In PHP, the array() function is used to create an array:

```php
<?php
//array();
$cars = array("Volvo", "BMW", "Toyota");//PHP Indexed Arrays
?>
```

In PHP, there are three types of arrays:<br/>
1.Indexed arrays - Arrays with a numeric index<br/>

```php
<?php
//array();
$cars = array("Volvo", "BMW", "Toyota");//PHP Indexed Arrays
?>
//loop array
for($x = 0; $x < $arrlength; $x++) {
  echo $cars[$x];
  echo "<br>";
}
```

2.Associative arrays - Arrays with named keys<br/>

```php
$age = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");
or
$age['Peter'] = "35";
$age['Ben'] = "37";
$age['Joe'] = "43";
foreach($age as $x => $value) {
  echo "Key=" . $x . ", Value=" . $value;
  echo "<br>";
}
```

3.Multidimensional arrays - Arrays containing one or more arrays<br/>

```php
$cars = array (
  array("Volvo",22,18),
  array("BMW",15,13),
  array("Saab",5,2),
  array("Land Rover",17,15)
);
//loop array:
for ($row = 0; $row < 4; $row++) {
  echo "<p><b>Row number $row</b></p>";
  echo "<ul>";
  for ($col = 0; $col < 3; $col++) {
    echo "<li>".$cars[$row][$col]."</li>";
  }
  echo "</ul>";
}
```

#### Methods in array

count(array)-used to return the length (the number of elements) of an array<br>
sort() - sort arrays in ascending order<br>
rsort() - sort arrays in descending order<br>
asort() - sort associative arrays in ascending order, according to the value<br>
arsort() - sort associative arrays in descending order, according to the value<br>
ksort() - sort associative arrays in ascending order, according to the key<br>
krsort() - sort associative arrays in descending order, according to the key<br>

## PHP Superglobal

### Super global variables are built-in variables that are always available in all scopes.

#### PHP $GLOBALS

$GLOBALS is a PHP super global variable which is used to access global variables from anywhere in the PHP script (also from within functions or methods).

PHP stores all global variables in an array called $GLOBALS[index]. The index holds the name of the variable.

The example below shows how to use the super global variable $GLOBALS:

```php
<?php
$x = 75;
$y = 25;

function addition() {
  $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
}

addition();
echo $z;
?>
```

#### PHP $\_SERVER

$\_SERVER is a PHP super global variable which holds information about headers, paths, and script locations.

The example below shows how to use some of the elements in $\_SERVER:

```php
<?php
echo $_SERVER['PHP_SELF'];
echo "<br>";
echo $_SERVER['SERVER_NAME'];
echo "<br>";
echo $_SERVER['HTTP_HOST'];
echo "<br>";
echo $_SERVER['HTTP_REFERER'];
echo "<br>";
echo $_SERVER['HTTP_USER_AGENT'];
echo "<br>";
echo $_SERVER['SCRIPT_NAME'];
?>
```

The result is:

```
/demo/demo_global_server.php
35.194.26.41
35.194.26.41
https://tryphp.w3schools.com/showphp.php?filename=demo_global_server
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:99.0) Gecko/20100101 Firefox/99.0
/demo/demo_global_server.php
```

<table class="ws-table-all notranslate">
<tbody><tr>
<th style="width:35%">Element/Code</th>
<th style="width:65%">Description</th>
</tr>
<tr>
<td>$_SERVER['PHP_SELF']</td>
<td>Returns the filename of the currently executing script</td>
</tr>
<tr>
<td>$_SERVER['GATEWAY_INTERFACE']</td>
<td>Returns the version of the Common Gateway Interface (CGI) the server is 
using</td>
</tr>
<tr>
<td>$_SERVER['SERVER_ADDR']</td>
<td>Returns the IP address of the host server</td>
</tr>
<tr>
<td>$_SERVER['SERVER_NAME']</td>
<td>Returns the name of the host server (such as www.w3schools.com)</td>
</tr>
<tr>
<td>$_SERVER['SERVER_SOFTWARE']</td>
<td>Returns the server identification string (such as Apache/2.2.24)</td>
</tr>
<tr>
<td>$_SERVER['SERVER_PROTOCOL']</td>
<td>Returns the name and revision of the information protocol (such as HTTP/1.1)</td>
</tr>
<tr>
<td>$_SERVER['REQUEST_METHOD']</td>
<td>Returns the request method used to access the page (such as POST)</td>
</tr>
<tr>
<td>$_SERVER['REQUEST_TIME']</td>
<td>Returns the timestamp of the start of the request (such as 1377687496)</td>
</tr>
<tr>
<td>$_SERVER['QUERY_STRING']</td>
<td>Returns the query string if the page is accessed via a query string</td>
</tr>
<tr>
<td>$_SERVER['HTTP_ACCEPT']</td>
<td>Returns the Accept header from the current request </td>
</tr>
<tr>
<td>$_SERVER['HTTP_ACCEPT_CHARSET']</td>
<td>Returns the Accept_Charset header from the current request (such as 
utf-8,ISO-8859-1)</td>
</tr>
<tr>
<td>$_SERVER['HTTP_HOST']</td>
<td>Returns the Host header from the current request </td>
</tr>
<tr>
<td>$_SERVER['HTTP_REFERER']</td>
<td>Returns the complete URL of the current page (not reliable because not all 
user-agents support it)</td>
</tr>
<tr>
<td>$_SERVER['HTTPS']</td>
<td>Is the script queried through a secure HTTP protocol</td>
</tr>
<tr>
<td>$_SERVER['REMOTE_ADDR']</td>
<td>Returns the IP address from where the user is viewing the current page</td>
</tr>
<tr>
<td>$_SERVER['REMOTE_HOST']</td>
<td>Returns the Host name from where the user is viewing the current page</td>
</tr>
<tr>
<td>$_SERVER['REMOTE_PORT']</td>
<td>Returns the port being used on the user's machine to communicate with the 
web server</td>
</tr>
<tr>
<td>$_SERVER['SCRIPT_FILENAME']</td>
<td>Returns the absolute pathname of the currently executing script</td>
</tr>
<tr>
<td>$_SERVER['SERVER_ADMIN']</td>
<td>Returns the value given to the SERVER_ADMIN directive in the web server 
configuration file (if your script runs on a virtual host, it will be the value 
defined for that virtual host) (such as someone@w3schools.com)</td>
</tr>
<tr>
<td>$_SERVER['SERVER_PORT']</td>
<td>Returns the port on the server machine being used by the web server for 
communication (such as 80)</td>
</tr>
<tr>
<td>$_SERVER['SERVER_SIGNATURE']</td>
<td>Returns the server version and virtual host name which are added to 
server-generated pages</td>
</tr>
<tr>
<td>$_SERVER['PATH_TRANSLATED']</td>
<td>Returns the file system based path to the current script</td>
</tr>
<tr>
<td>$_SERVER['SCRIPT_NAME']</td>
<td>Returns the path of the current script</td>
</tr>
<tr>
<td>$_SERVER['SCRIPT_URI']</td>
<td>Returns the URI of the current page</td>
</tr>
</tbody></table>

#### PHP $\_REQUEST

PHP $\_REQUEST is a PHP super global variable which is used to collect data after submitting an HTML form.

The example below shows a form with an input field and a submit button. When a user submits the data by clicking on "Submit", the form data is sent to the file specified in the action attribute of the \<form> tag. In this example, we point to this file itself for processing form data. If you wish to use another PHP file to process form data, replace that with the filename of your choice. Then, we can use the super global variable $\_REQUEST to collect the value of the input field:<br>
Here's an example:

```php
<html>
<body>

<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
  // collect value of input field
  $name = $_REQUEST['fname'];
  if (empty($name)) {
    echo "Name is empty";
  } else {
    echo $name;
  }
}
?>

</body>
</html>

```

#### PHP $\_POST

Both \_POST and $\_REQUEST are the super global variable, and they are designed to be retrive the values in matching method, and here's a simple example:

```php
<html>
<body>
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
  Name: <input type="text" name="fname">
  <input type="submit">
</form>

<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
  // collect value of input field
  $name = $_POST['fname'];
  if (empty($name)) {
    echo "Name is empty";
  } else {
    echo $name;
  }
}
?>
</body>
</html>
```

#### PHP $\_GET

PHP $\_GET is a PHP super global variable which is used to collect form data after submitting an HTML form with method="get".

$\_GET can also collect data sent in the URL.

Assume we have an HTML page that contains a hyperlink with parameters:

```php
<html>
<body>

<a href="test_get.php?subject=PHP&web=W3schools.com">Test $GET</a>

</body>
</html>
```

When a user clicks on the link "Test $GET", the parameters "subject" and "web" are sent to "test_get.php", and you can then access their values in "test_get.php" with $\_GET.

The example below shows the code in "test_get.php":

```php
<html>
<body>

<?php
echo "Study " . $_GET['subject'] . " at " . $_GET['web'];
?>

</body>
</html>
```

## Regular Expression

A regular expression is a sequence of characters that forms a search pattern. When you search for data in a text, you can use this search pattern to describe what you are searching for.

A regular expression can be a single character, or a more complicated pattern.

Regular expressions can be used to perform all types of text search and text replace operations.

In PHP, regular expressions are strings composed of delimiters, a pattern and optional modifiers.

```php
$exp = "/w3schools/i";
```

In the example above, / is the delimiter, w3schools is the pattern that is being searched for, and i is a modifier that makes the search case-insensitive.

The delimiter can be any character that is not a letter, number, backslash or space. The most common delimiter is the forward slash (/), but when your pattern contains forward slashes it is convenient to choose other delimiters such as # or ~.

#### Regular Expression Functions

<table>
<th>Function</th>
<th>Description</th>
<tr>
<td>preg_match(pattern,target)</td>
<td>Returns 1 if the pattern was found in the string and 0 if not</td>
</tr>
<tr>
<td>preg_match_all(pattern,target)</td>
<td>Returns the number of times the pattern was found in the string, which may also be 0</td>
</tr>
<tr>
<td>preg_replace(pattern,replacement,target)</td>
<td>Returns a new string where matched patterns have been replaced with another string</td>
</tr>
</table>

#### Regular Expression Modifiers

<table>
<th>Modifer</th>
<th>Description</th>
<tr>
<td>i</td>
<td>Performs a case-insensitive search</td>
</tr>
<tr>
<td>m</td>
<td>Performs a multiline search (patterns that search for the beginning or end of a string will match the beginning or end of each line)</td>
</tr>
<tr>
<td>u</td>
<td>Enables correct matching of UTF-8 encoded patterns</td>
</tr>
</table>

<table>
<th>Expression</th>
<th>Description</th>
<tr>
<td>[abc]</td>
<td>Find one character from the options between the brackets</td>
</tr>
<tr>
<td>[^abc]</td>
<td>Find any character NOT between the brackets</td>
</tr>
<tr>
<td>[0-9]</td>
<td>Find one character from the range 0 to 9</td>
</tr>
</table>

<table>
<th>Matacharacter</th>
<th>Description</th>
<tr>
<td>|</td>
<td>Find a match for any one of the patterns separated by | as in: cat|dog|fish</td>
</tr>
<tr>
<td>.</td>
<td>Find a match for any one of the patterns separated by | as in: cat|dog|fish</td>
</tr>
<tr>
<td>^</td>
<td>Finds a match as the beginning of a string as in: ^Hello</td>
</tr>
<tr>
<td>$</td>
<td>Finds a match at the end of the string as in: World$</td>
</tr>
<tr>
<td>\d</td>
<td>Find a digit</td>
</tr>
<tr>
<td>\s</td>
<td>Find a whitespace character</td>
</tr>
<tr>
<td>\b</td>
<td>Find a match at the beginning of a word like this: \bWORD, or at the end of a word like this: WORD\b</td>
</tr>
<tr>
<td>\uxxxx</td>
<td>Find the Unicode character specified by the hexadecimal number xxxx</td>
</tr>
</table>

#### Quantifiers

Quantifiers define quantities:

<table>
<th>Quantifier</th>
<th>Description</th>
<tr>
<td>n+</td>
<td>Matches any string that contains at least one n</td>
</tr>
<td>n*</td>
<td>Matches any string that contains zero or more occurrences of n</td>
</tr>
<tr>
<td>n?</td>
<td>Matches any string that contains zero or one occurrences of n</td>
</tr>
<tr>
<td>n{x}</td>
<td>Matches any string that contains a sequence of X n's</td>
</tr>
<tr>
<tr>
<td>n{x,y}</td>
<td>Matches any string that contains a sequence of X to Y n's</td>
</tr>
<tr>
<td>n{x,}</td>
<td>Matches any string that contains a sequence of at least X n's</td>
</tr>
</table>

#### PHP Forms

We have learned when we want retrive the data from client-side in the server, we could use

```php
$_GET['name']
```

to get the data sent with GET method and use

```php
$_POST['name']
```

to get the data sent with POST method.<br>
Now we start to learn the PHP Form Validation:

### Method: htmlspecialchars(str)

This method is foremost important for preventing XSS attacks. So what is a XSS attack? Fore Example:

```php
/*It is a server page:*/
<?php
// define variables and set to empty values
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $name = $_POST["name"];
  $email = $_POST["email"];
  $website = $_POST["website"];
  $comment = $_POST["comment"]);
  $gender = $_POST["gender"];
}

function test_input($data) {
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
?>

<?php
echo "<h2>Your Input:</h2>";
echo $name;
echo "<br>";
echo $email;
echo "<br>";
echo $website;
echo "<br>";
echo $comment;
echo "<br>";
echo $gender;
?>
```

This is a client-side page:

```php
<!DOCTYPE HTML>
<html>
<head>
</head>
<body>
<h2>PHP Form Validation Example</h2>
<form method="post" action="<?php echo "target.php"?>">
  Name: <input type="text" name="name">
  <br><br>
  E-mail: <input type="text" name="email">
  <br><br>
  Website: <input type="text" name="website">
  <br><br>
  Comment: <textarea name="comment" rows="5" cols="40"></textarea>
  <br><br>
  Gender:
  <input type="radio" name="gender" value="female">Female
  <input type="radio" name="gender" value="male">Male
  <input type="radio" name="gender" value="other">Other
  <br><br>
  <input type="submit" name="submit" value="Submit">
</form>

</body>
</html>
```

When a hacker add a string:"\<script>alert("Hello")\</script>" after your input . After you submit the form to the server, this script will be executed. And you will find this alert on the page.But when you use method:
test_input($data) everything will change:

```php
<?php
// define variables and set to empty values
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $name = test_input($_POST["name"]);
  $email = test_input($_POST["email"]);
  $website = test_input($_POST["website"]);
  $comment = test_input($_POST["comment"]);
  $gender = test_input($_POST["gender"]);
}

function test_input($data) {
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
?>

<?php
echo "<h2>Your Input:</h2>";
echo $name;
echo "<br>";
echo $email;
echo "<br>";
echo $website;
echo "<br>";
echo $comment;
echo "<br>";
echo $gender;
?>
```

Beacause when you are using a htmlspecialchars, all the <>will become &lt and &gt, so the script won't be executed.

##### Note:When we are using $\_SERVER["PHP_SELF"], we need add a htmlspecialchars too in order to protect the submit.

And when we want to set the Required attribute, we could do something like this:

```php
<?php
// define variables and set to empty values
$nameErr = $emailErr = $genderErr = $websiteErr = "";
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  if (empty($_POST["name"])) {
    $nameErr = "Name is required";
  } else {
    $name = test_input($_POST["name"]);
  }

  if (empty($_POST["email"])) {
    $emailErr = "Email is required";
  } else {
    $email = test_input($_POST["email"]);
  }

  if (empty($_POST["website"])) {
    $website = "";
  } else {
    $website = test_input($_POST["website"]);
  }

  if (empty($_POST["comment"])) {
    $comment = "";
  } else {
    $comment = test_input($_POST["comment"]);
  }

  if (empty($_POST["gender"])) {
    $genderErr = "Gender is required";
  } else {
    $gender = test_input($_POST["gender"]);
  }
}
?>
<?php
function test_input($data){
  $data=htmlspecialchars($data);
  return $data;
}
?>
<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">

Name: <input type="text" name="name">
<span class="error">* <?php echo $nameErr;?></span>
<br><br>
E-mail:
<input type="text" name="email">
<span class="error">* <?php echo $emailErr;?></span>
<br><br>
Website:
<input type="text" name="website">
<span class="error"><?php echo $websiteErr;?></span>
<br><br>
Comment: <textarea name="comment" rows="5" cols="40"></textarea>
<br><br>
Gender:
<input type="radio" name="gender" value="female">Female
<input type="radio" name="gender" value="male">Male
<input type="radio" name="gender" value="other">Other
<span class="error">* <?php echo $genderErr;?></span>
<br><br>
<input type="submit" name="submit" value="Submit">

</form>
```

#### Date and Time

The PHP date() function formats a timestamp to a more readable date and time.

```php
date(format,[timestamp])
```

The required format parameter of the date() function specifies how to format the date (or time).

Here are some characters that are commonly used for dates:

d - Represents the day of the month (01 to 31)
m - Represents a month (01 to 12)
Y - Represents a year (in four digits)
l (lowercase 'L') - Represents the day of the week
Other characters, like"/", ".", or "-" can also be inserted between the characters to add additional formatting.

The example below formats today's date in three different ways:

```php
echo "Today is " . date("Y/m/d") . "<br>";
echo "Today is " . date("Y.m.d") . "<br>";
echo "Today is " . date("Y-m-d") . "<br>";
echo "Today is " . date("l");
```

#### Get a Time

Here are some characters that are commonly used for times:

H - 24-hour format of an hour (00 to 23)
h - 12-hour format of an hour with leading zeros (01 to 12)
i - Minutes with leading zeros (00 to 59)
s - Seconds with leading zeros (00 to 59)
a - Lowercase Ante meridiem and Post meridiem (am or pm)
The example below outputs the current time in the specified format:

```php
<?php
echo "The time is " . date("h:i:sa");
?>
```

#### Get Your Time Zone

If the time you got back from the code is not correct, it's probably because your server is in another country or set up for a different timezone.

So, if you need the time to be correct according to a specific location, you can set the timezone you want to use.

The example below sets the timezone to "America/New_York", then outputs the current time in the specified format:

```php
<?php
date_default_timezone_set("America/New_York");
echo "The time is " . date("h:i:sa");
?>
```

Create a Date With mktime()
The optional timestamp parameter in the date() function specifies a timestamp. If omitted, the current date and time will be used (as in the examples above).

The PHP mktime() function returns the Unix timestamp for a date. The Unix timestamp contains the number of seconds between the Unix Epoch (January 1 1970 00:00:00 GMT) and the time specified.

```php
mktime(hour, minute, second, month, day, year)
```

The example below creates a date and time with the date() function from a number of parameters in the mktime() function:

```php
<?php
$d=mktime(11, 14, 54, 8, 12, 2014);
echo "Created date is " . date("Y-m-d h:i:sa", $d);
?>
```

#### Create a Date From a String With strtotime()

The PHP strtotime() function is used to convert a human readable date string into a Unix timestamp (the number of seconds since January 1 1970 00:00:00 GMT).

```php
strtotime(time, now)
```

Example:

```php
<?php
$d=strtotime("10:30pm April 15 2014");
echo "Created date is " . date("Y-m-d h:i:sa", $d);
?>
```

PHP is clever to convert the date string:

```php
<?php
$d=strtotime("tomorrow");
echo date("Y-m-d h:i:sa", $d) . "<br>";

$d=strtotime("next Saturday");
echo date("Y-m-d h:i:sa", $d) . "<br>";

$d=strtotime("+3 Months");
echo date("Y-m-d h:i:sa", $d) . "<br>";
?>
```

#### Include in PHP

The include (or require) statement takes all the text/code/markup that exists in the specified file and copies it into the file that uses the include statement.

Including files is very useful when you want to include the same PHP, HTML, or text on multiple pages of a website.<br>
Format:

```php
include 'filename';

or

require 'filename';
```

Here's an example:

```php
<?php
echo "<p>Copyright &copy; 1999-" . date("Y") . " W3Schools.com</p>";
?>
```

We assume the code above is in the file called footer.php

```php
<html>
<body>

<h1>Welcome to my home page!</h1>
<p>Some text.</p>
<p>Some more text.</p>
<?php include 'footer.php';?>
//Now we have included the code above into here
</body>
</html>
```

And we mentioned that expect Include we also have require to include the file into the page, but they have a big difference: When the include file is not found, use include, the script will continue to work, but when use the require, the script below will not work anymore.

#### ReadFile(path)

Use this file we could get all the content of the file in txt and at the end of the text file will display the total bytes of the file

To open/read a file we can also use fopen() which has two parameters: one for the filename and another for options, here's all the options:

<table>
<th>Modes</th>
<th>Description</th>
<tr>
<td>r</td>
<td>Open a file for read only. File pointer starts at the beginning of the file</td>
</tr>
<tr>
<td>r+</td>
<td>Open a file for read/write. File pointer starts at the beginning of the file</td>
</tr>
<tr>
<td>w</td>
<td>Open a file for write only. Erases the contents of the file or creates a new file if it doesn't exist. File pointer starts at the beginning of the file</td>
</tr>
<tr>
<td>w+</td>
<td>Open a file for read/write. Erases the contents of the file or creates a new file if it doesn't exist. File pointer starts at the beginning of the file</td>
</tr>
<tr>
<td>a</td>
<td>Open a file for write only. The existing data in file is preserved. File pointer starts at the end of the file. Creates a new file if the file doesn't exist</td>
</tr>
<tr>
<td>a+</td>
<td>Open a file for read/write. The existing data in file is preserved. File pointer starts at the end of the file. Creates a new file if the file doesn't exist</td>
</tr>
<tr>
<td>x</td>
<td>Creates a new file for write only. Returns FALSE and an error if file already exists</td>
</tr>
<tr>
<td>x+</td>
<td>Creates a new file for read/write. Returns FALSE and an error if file already exists</td>
</tr>
</table>

#### PHP Read File - fread()

The fread() function reads from an open file.

The first parameter of fread() contains the name of the file to read from and the second parameter specifies the maximum number of bytes to read.

The following PHP code reads the "webdictionary.txt" file to the end:

```php
fread($myfile,filesize("webdictionary.txt"));
```

##### filesize(file):return size of the file

After we manipulate the file, we shou use fclose(filename) to close the file to prevent it using the resources of the server.

```php
<?php
$myfile = fopen("webdictionary.txt", "r");
// some code to be executed....
fclose($myfile);
?>
```

#### fgets(file)

This method is used to read the single line from the file, so in order to read the whole file we need use an another method called feof(file) that will check if the "end-of-file"(EOF) has been reached. And here's an example for that:

```php
<?php
$myfile = fopen("webdictionary.txt", "r") or die("Unable to open file!");
// Output one line until end-of-file
while(!feof($myfile)) {
  echo fgets($myfile) . "<br>";
}
fclose($myfile);
?>
```

#### PHP Read Single Character - fgetc():

With this function we could read a single character of the file;

#### File create and write in PHP:

When we are using fopen(), if there is no such file, it will be created. So we create a new file with the function fopen() too.
But when we need to write this new file, we need to use method:
fwrite(file,content).

#### PHP Overwriting and Append

When we finished writing a new file. If we start writing it again, besides the fopen()'s option is "w", the initial data will all be overwrited, we call it Overwriting. And when you choose the option
"a", the initial data will be preserved.

#### File Upload:

Beacause the file upload is a little complicated, so we firstly showcase a example here:

```php
//index page
<!DOCTYPE html>
<html>
<body>

<form action="upload.php" method="post" enctype="multipart/form-data">
  Select image to upload:
  <input type="file" name="fileToUpload" id="fileToUpload">
  <input type="submit" value="Upload Image" name="submit">
</form>

</body>
</html>
```

```php
//upload page
<?php
$target_dir = "uploads/";
$target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]);
//basename:eturns the filename from a path
//$_FILES["filename",["name"]],get the file  uploaded by post, we could use filename to get the file like this,
//we can also use type or size ect to get the file too, like when we use the type:$_FILES["filetype",["type"]]
$uploadOk = 1;
$imageFileType = strtolower(pathinfo($target_file,PATHINFO_EXTENSION));
// Check if image file is a actual image or fake image
if(isset($_POST["submit"])) {
//The isset function in PHP is used to determine whether a variable is set or not
  $check = getimagesize($_FILES["fileToUpload"]["tmp_name"]);
  if($check !== false) {
    echo "File is an image - " . $check["mime"] . ".";
    $uploadOk = 1;
  } else {
    echo "File is not an image.";
    $uploadOk = 0;
  }
}

// Check if file already exists
if (file_exists($target_file)) {
  echo "Sorry, file already exists.";
  $uploadOk = 0;
}

// Check file size
if ($_FILES["fileToUpload"]["size"] > 500000) {
  echo "Sorry, your file is too large.";
  $uploadOk = 0;
}

// Allow certain file formats
if($imageFileType != "jpg" && $imageFileType != "png" && $imageFileType != "jpeg"
&& $imageFileType != "gif" ) {
  echo "Sorry, only JPG, JPEG, PNG & GIF files are allowed.";
  $uploadOk = 0;
}

// Check if $uploadOk is set to 0 by an error
if ($uploadOk == 0) {
  echo "Sorry, your file was not uploaded.";
// if everything is ok, try to upload file
} else {
  if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
    echo "The file ". htmlspecialchars( basename( $_FILES["fileToUpload"]["name"])). " has been uploaded.";
  } else {
    echo "Sorry, there was an error uploading your file.";
  }
}
?>

```

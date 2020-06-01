<img src="icon.png" align="right" />

# RevoEngine
RevoEngine is a java-scripting engine which opens up your program and allows users to create their own scripts using "Revo", which is a fancy word for a custom language which is heavily inspired by Java.

The goal of RevoEngine was to allow users to script a game or program and introduce code on-the-fly without having to restart the application and in a sandboxed and safe environment. Furthermore the goal of RevoEngine is that it should work on android, desktop, headless environments and any other platform supporting Java. As such, the usage of reflection should be kept to an absolute minimum.

Another of the must-have goals was for the generated code to be able to extend Java classes. This is made possible by generating all the code required for Revo scripts to attach to Java ahead of time using a custom "RevoGenerator" tool.

## RevoScript Language

The RevoScript language functions very similar to Java, with a few additions and removals here and there. Here are all of the language features:

### Comments

Comments can be placed anywhere within a script and have two variations:
* Single-line comment: `//`
  * This defines the rest of the line as a comment
  * The compiler will ignore the remainder of the line after these two symbols
* Multi-line comment: `/* */` 
  * Using this comment will make the compiler start ignoring code when it encounters the `/*` symbols, and skip over everything until the `*/` symbols are encountered.

### Classes

Classes are containers which can contain functionality (methods) and data (fields)!
The class declaration syntax is as follows:
```
MyClass {
}
```
So simply providing the name of the class, followed by a block is all you need!

In order to instantiate a class (create a new object from it), you simply use the new operator as such: `new MyClass();`
Depending on which parameters you put into the round brackets `()` you will call a different constructor! If there is no constructor specified, then the class will automatically have a no-argument constructor generated.


#### Class Fields

Classes have so-called fields which can be used to store objects for later retrieval.
To define a class field simply specify a type, followed by the name you want to assign to the field. Here is an example:
```
MyClass {
 float myFloat = 0.1F;
 int secondValue;
}
```
If you do not assign a value to the field, it will either be `null` for Objects or the default value for primitive types (see primitive type definitions further below).

In order to obtain a field value from a class, simply create a new object of the class and then access the field by using a dot and then the field name. Here is an example: 
```
MyClass created = new MyClass();
created.myFloat = 4.0F;
print created.myFloat;
```
This code will first create an instance of MyClass and assign it to the field with the name `created`. Then, it will set the value of the field `myFloat` inside `created` to 4.0. Finally, the value of `myFloat` inside `created` will be printed to the console.

#### Class methods

An object of a certain class can execute certain functionality by defining methods.
To define a method first write the type of value you want, followed by the method name and the parameters of a method in round brackets. Finally, end it with the code block to be executed as follows:
```
MyClass {

 int getNumberThree() {
  return 3;
 }

}
```

This method can then be called by executing it on an instance of the defining class. Here is an example:
```
MyClass test = new MyClass();
print test.getNumberThree();
```
This will simply create a new MyClass value and assign it to the field called `test`. Then, it will call the method inside MyClass and print the returned value (in our case, 3).

If you do not want the method to return a value, simply set its return type to `void`. You can then end method execution by simply executing `return;`. Here is an example:
```
MyClass {

 boolean endEarly = true;

 void voidMethod() {
  if (endEarly) {
   print "Early stop!";
   return;
  }
  print "Normal execution!";
 }
 
}
```
In this example, the class will print "Normal Execution!" to the console only if we did not set the `endEarly` field to `true` after creating an object of the class `MyClass`. An example using both:
```
MyClass executions = new MyClass();
executions.voidMethod();
executions.endEarly = false;
executions.voidMethod();
```
This code will first print "Early stop!", followed by "Normal execution!".

If you want to only define methods but not give it any code, simply replace the brackets with a semicolon like so:
```
MyClass {
 void myMethod(int parameter);
}
```

#### Object inheritance, super classes

Any non-interface class can extend another class by specifying the to-extend class in round brackets after the class name. Here is an example: `MyClass (ArrayList) {...`

This will inherit all fields, methods and interfaces from the superclass!

#### Interfaces

A class can be declared as an interface by specifying `interface` before the class name. The class declaration would then look as such: `interface MyInterface {...`

Interfaces are a special type of class. They cannot introduce class fields or constructors, they can however introduce new methods to the classes which implement them.

An interface can be implemented by a class by appending `:` followed by a list of interfaces to append. 
Here is an example: `MyClass : MyInterface, Comparable`

One can implement as many interfaces as one wishes. Classes which inherit from interfaces must ensure that all methods of the implemented interfaces with undefined bodies are declared!

#### Overriding methods

When a method matches the signature of a method defined in one of the superclasses or implementing interfaces, the `override` keyword should be attached to the method in order to signify that this method was defined in those already and you're changing the method's behavior. 

While not doing this will result in only a warning instead of an error, it is vital as the syntax of a script can become much clearer due to it.
As an example, let's think of the class `Foo` which defines the method `void myMethod(int test)`. If we have a class `Bar` extending `Foo` we can override the method using the following syntax:
```
override void myMethod(int test){
  super.myMethod(test);
  print "Test was executed: " + test;
}
```
If you want to execute the original method's code, simply precede the method with `super.`

### Arrays

Arrays are a fixed-size collection of values which can be accessed using an index. An array can be defined by appending [] after defining the class type. For example: `int[] numbers;`
Arrays have a set size and cannot be shrunk or grown mid-runtime. To access an array's element, simply call the array name, followed by an index surrounded by `[]` brackets (f.e.: `myArray[0]`, `myArray[i]` or `myArray[getIndex()`).
When declaring an array, it can be initialized in three possible ways:
* Sized initialization:
  * Example: `int[] myArray = new int[4]`
  * This will create a new empty array with the specified size.
  * The values inside the array will either have the default primitive value, or null
* Declaring initialization: 
  * This type of declaration predefines all the values inside the arrays, simultaneously with its size
  * This can have two possible syntaxes:
    * When defining a field: `int[] myField = {0, 1, 2, 3};`
    * When calling a method `myArrayMethod(new int[] {0, 1, 2, 3});`
  * The latter syntax for array initalization can also be used for field declarations, however the former cannot be using when calling methods, as methods with the same names might require different types of arrays.

### Object Casting

Revo was designed with simplicity in mind. As such, you do not need to cast from the Object class into any other class. However, casting a class to an inheriting class is otherwise still required. Here are some examples of where casts are needed and where not:
```
Object test = "A String-type value!";
String testString = test; // No cast needed! We are casting from Object to String
Foo foo = new Bar(); // Foo is an interface implemented by Bar
Bar bar = (Bar) foo; // We need a cast here, in order to prevent coding mishaps
```

### Operator Declaration

TODO 

### Lambdas

If an interface defines only a single method, one can create a simple implementation of it by pointing to a method matching the method's signature (a signature being the target method's return value and parameter types). The syntax for this is to provide an object, followed by `::` and the method which should act as interface's target method body.
Here is an example:
```
interface LambdaSample {
  boolean doSomething(int parameter);
}
```
This is our interface. In order to create a type of it we can do the following in a class:
```
MyClass {
 
 new() {
  LambdaSample conformIt = this::lambdaTest;
  conformIt.doSomething(0);
 }
 
 boolean lambdaTest(int param) {
  print "Executed doSomething with parameter: " + param;
  return true;
 }
}
```
In this sample, we first create a type of `LambdaSample` from the definition of our `lambdaTest` method. This will basically create a hidden class which simply calls `lambdaTest` in our object when `doSomething` is called.

### Loops
 
Loops can be used to repeat a specific block of code as many times as you need it to be repeated. There are various types of loops that you can use through your code based on your needs:

#### Do / While
 
This type of loop will execute the code inside the block first, before checking if the execution should be repeated at the end.
The syntax is as follows:
```
do {
 // Your code here
} while (booleanValue);
```
After the code was executed and `booleanValue` returns false, the loop stops and code execution continues.

#### While

This type of loop will first check the condition before executing the code inside the block. Here is a sample:
```
while (booleanValue) {
 // Your code here
}
```
If `booleanValue` returns true, then the loop starts execution. When the block ends and `booleanValue` is still true, the loop restarts.

#### For

A for loop consists of three parameters separated by semi-colons (variable initializers, condition and statement) followed by a block body or a statement.
Here is a typical example of a for loop which iterates through an array and prints out each value inside: 

```
for(int i = 0; i < myArray.length; i++) {
 // Your code
}
```

This loop is executed as follows:
1. The first parameter defines a variable `i` and assigns it the value 0 (`int i=0;`). 
2. The condition is checked whether the iteration should start (`i < myArray.length;`). 
3. The statement or block of the loop is executed.
4. After the statement is completed, the third parameter (`i++`) is executed. 
5. Then, if the condition is still met (`i < myArray.length`) we go back to step 3!

#### Enhanced For loop

If you want to iterate over an object implementing the `Iterable` interface (f.e. ArrayList, HashSet, HashMap) or any array and do not care about the index, simply use the enhanced for loop. This can be done as follows:
```
for(Object obj : myIterable) {
 // Your code
}
```
This will automatically go through the entire iterable object and always feed the next element's value into the `obj` variable. Make sure to make the type of the `obj` fit to whatever class values are present inside the `myIterable` object. 

#### Continue / Break

If you want to interrupt the current loop or restart the execution, you can either use `break` or `continue`. Both of these commands immediately stop the loop execution and end the loop block, however `break` will stop the loop entierly, whereas `continue` will instead restart the loop body if the loop condition is still met (and execute the for-statement update).

### Exception throwing / catching

During program execution, certain exceptions might be thrown 
Exception can be caught by using a try/catch block. This executes the code inside the block and if an Exception is thrown which is one of the types specified inside the catch block. Here is a typical try/catch block: 
```
try {
 int test = Integer.parseInt(someString);
} catch (NumberFormatException e){
 print "Could not format string to number!";
}
```

If the function parseInt throws a NumberFormatException, then the currently executing block will be immediately interrupted, and the block following the catch clause with the print function will be executed. 

### Class "attaching"

Certain classes can be "attached" onto a program. What this means can vary depending on the program or game using RevoScript.

An example in EndCycle VS would be to attach a new attack pattern class to the list of available attack patterns. This can be done by adding the prefix `attach` before the class declaration as follows: `attach NewAttack (Attack) {...`

#### Attach block

After a class is attached and the program has loaded all the requirements for it, the class' `attached{...}` block is executed. This is an optional block which attaching classes can define **an the very top** in order to load specific features after initialization is completed. An example would be to load a player's inventory only after the item definitions were loaded. Here is an example:
```
attach MyListener : GameEventListener {

  attached {
    inventory = GameData.load("inventory.json");
  }
  
  CustomInventory inventory;
  
  new() {
  }
}
```
In here, the Listener object is created first and attached to the program. Then, after all of the item definitions are finished loading the attached block is called.

### Debugging

When debugging a script, two language features can be employed: `print` and `debug;`.
* The `print` function is a language-level method which takes a string or object and prints it out to the console log of the application.
  * This can be used to debug and track specific object values as they change
  * Example: `print this.counter;` would print the value of a field called counter within the current class
* `debug;` stops the execution of the script and displays information about the current local variables, the current calling stack as well as the values present inside the current class.  


### List of unsupported features compared to Java

While Revo is very similar to Java, some elements which are a core part of Java are not present in the language. Here are some major elements which Revo does not support in order to enforce scripting simplicity:
* Switch statements
* Visibility modifiers (`public`, `protected`, `private`)
* Synchronized and volatile modifiers
* Annotations
* Generics
* Anonymous classes

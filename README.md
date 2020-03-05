# RevoEngine
RevoEngine is a java-scripting engine which opens up your program and allows users to create their own scripts using "RevoScript", which is a fancy word for a language that supports a subset of java instructions.

The goal of RevoEngine was to allow users to script a game or program and introduce code on-the-fly without having to restart the application. Furthermore RevoEngine should work on android, desktop, headless environments and any other platform supporting Java. As such, there should be as little usage of reflection as possible.

Another of the must-have goals was for the generated code to be able to extend Java classes. This was made possible by using the Adapter pattern.

## RevoScript Language

The RevoScript language functions very similar to Java, with a few additions and removals here and there. Here are all of the language features:

### Variable Declaration

A variable can be simply declared using `Type variable;`

Variables can immediately get a value assigned to them, for example `int foo = 3;`

### Class Declaration

The class declaration syntax is as follows:
```
MyClassName {
  
  Type1 foo;
  Type2 bar;
  
  new(Type1 constructorParameter) {
    this.foo = constructorParameter;
    methodOne();
  }
  
  void methodOne() {
    print("Accessed methodOne!");
  }
  
  boolean compareFoo(Type1 comparing) {
    return foo == comparing;
  }
}
```

### Interfaces

A class can be declared as an interface by specifying `interface` before the class name. The class declaration would then look as such: `interface MyInterface {...`

Interfaces cannot introduce new class fields or constructors, they can however introduce new methods.  Method bodies can either be defined or undefined in an interface as follows:
```interface MyInterface {

  void count(int number) {
    for(int i=0; i < number; i++) {
      if (shouldPrint(number)) {
        print(number);
      }
    }
  }
  
  boolean shouldPrint(int number);

}```

An interface can be implemented by a class by appending `:` followed by a list of interfaces to append. Here is an example: `MyClass : MyInterface, Comparable`

When an interface is implemented, the implementing class must ensure that all methods with undefined bodies are specified.

### Object inheritance, super classes

Any non-interface class can extend another class by specifying the to-extend class in round brackets after the class name. Here is an example: `MyClass (ArrayList) {...`

### Class "attaching"

Certain classes can be "attached" onto a program. What this means can vary depending on the program or game using RevoScript.

An example in EndCycle VS would be to attach a new attack pattern class to the list of available attack patterns. This can be done by adding the prefix `attach` before the class declaration as follows: `attach NewAttack (Attack) {...`

Note: A class must not be an interface in order to be attachable.

### List of unsupported features

* Switch statements
* Visibility modifiers (`public`, `protected`, `private`, etc.)
* "Override" keyword
* Annotations
* Generics


**Variables** are used to store information to be referenced and manipulated in a computer program. They also provide a way of labeling data with a descriptive name, so our programs can be understood more clearly by the reader and ourselves. It is helpful to think of variables as containers that hold information. Their sole purpose is to label and store data in memory. This data can then be used throughout your program.

### Constant declaration

Constants are declared by capitalizing every letter in the variable's name, per Ruby convention. They are used for storing data that never needs to change. While most programming languages do not allow you to change the value assigned to a constant, Ruby does. It will however throw a warning letting you know that there was a previous definition for that variable. Just because you can, doesn't mean you should change the value. In fact, you should not. Constants cannot be declared in method definitions, and are available throughout your application's scopes.
```ruby
MY_CONSTANT = 'I am available throughout your app.'
```

### Global variable declaration

Global variables are declared by starting the variable name with the dollar sign (`$`). These variables are available throughout your entire app, overriding all scope boundaries. Rubyists tend to stay away from global variables as there can be unexpected complications when using them.
```ruby
$var = "I am also available throughout your app"
```

### Class variable declaration

Class variables are declared by starting the variable name with two `@` signs. These variables are accessible by instances of your class, as well as the class itself. When you need to declare a variable that is related to a class, but each instance of that class does not need its own value for this variable, you use a class variable. Class variables must be initialized at the class level, outside of any method definitions. They can then be altered using class or instance method definitions.
```ruby
@@instances = 0
```

### Instance variable declaration

Instance variables are declared by starting the variable name with one `@` sign. These variables are available throughout the current instance of the parent class. Instance variables can cross some scope boundaries, but not all of them. You will learn more about this when you get to OOP topics, and should not use instance variables until you know more about them.
```ruby
@var = 'I am available throughout the current instance of this class.'
```

### Local variable declaration

Local variables are the most common variables you will come across and obey all scope boundaries. These variables are declared by starting the variable name with neither `$` nor `@`, as well as not capitalizing the entire variable name.
```ruby
var = 'I must be passed around to cross scope boundaries.'
```


# C# Fundamentals

## 🔍 What is C#

C# (pronounced "C-sharp") is a modern, object-oriented programming language developed by [Microsoft](https://www.microsoft.com/). It was first introduced in 2000 and has evolved into one of the most versatile and widely used programming languages since. C# is designed to be simple, yet powerful, making it an excellent choice for both beginners and experienced developers.

### Key Features

1. **Object-Oriented**: C# supports all the core concepts of OOP ([object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming)) such as encapsulation, inheritance and polymorphism, which help in creating modular and reuseable code.
1. **Type-Safe**: C# enforces strict type checking, reducing the chances of runtime errors and enhancing code reliability.
1. **Rich Library**: It provides a comprehensive standard library that simplifies many programming tasks, including file I/O, data manipulation and networking.
1. **Component-Oriented**: C# is designed for creating software components, which are reusable code modules that can be easily integrated into larger systems.
1. **Integrated with .NET Framework**: It has seamless integration with the [.NET framework](https://dotnet.microsoft.com) providing access to a vast array of libraries and tools for building robust applications.

### Uses of C#

1. **Desktop Applications**: C# is widely used to develop Windows desktop applications using frameworks like Windows Forms and WPF (Windows Presentation Foundation).
1. **Web Applications**: [ASP.NET](https://dotnet.microsoft.com/en-us/apps/aspnet), a powerful framework for building web applications, is primarily based on C#. Developers can create dynamic websites, web APIs, and services with ASP.NET.
1. **Mobile Applications**: With [Xamarin](https://dotnet.microsoft.com/en-us/apps/xamarin), a cross-platform mobile development framework, developers can write C# code to build native Android, iOS, and Windows mobile applications.
1. **Game Development**: C# is the primary language for Unity, one of the most popular game engines. It is used extensively in the development of both 2D and 3D games.
1. **Cloud and Enterprise Applications**: C# is commonly used to develop scalable cloud applications and enterprise-level software, particularly in environments that use Microsoft Azure.
1. **IoT Applications**: C# can also be used for developing Internet of Things (IoT) applications, leveraging its ability to interface with hardware and communicate over networks.
   Whatever the goal is, there most likely is a way to do it with C#.

## 📝 Basic Synctax and Structure

### ~~Main Method~~ (Deprecated)

Since C# 10 there is no more need for a `static void Main(string args[])` method. Instead you can create a new file that contains `Console.WriteLine("Hello World");` and run it without further code.

### Variables and Data Types

The common data types are the following

```c#
// INT for whole numbers
int age = 30;

// FLOAT for floating point numbers
float height = 5.9f;

// DOUBLE for double-precision floating point numbers
double weight = 70.5;

// CHAR for single characters
char grade = 'A';

// STRING for words / text
string name = "John";

// BOOL for true or false (boolean)
bool isStudent = true;
```

A variable is always declared as seen. [TYPE / "var"] [VARIABLE_NAME] [(optional) = [VALUE / VALUABLE EXPRESSION]]
The `var` keyword can be used instead of any type keyword. Be careful with `var` as it might get unclear what the type the variable is quickly.

### Conditional Statements & Loops

#### If / Else Conditions

```c#
int age = 17;
if (age < 18)
{
    Console.WriteLine("You are too young to do this.");
}
else
{
    Console.WriteLine("You are old enough.");
}

// Short Form: Ternary Operator: [BOOLEAN] ? [TRUE] : [FALSE]
age < 18
    ? Console.WriteLine("You are too young to do this.");
    : Console.WriteLine("You are old enough.");
```

#### Switch Statements

If there are more than 2 possible outcomes that you want to handle a switch statement can be used. Switch statements should be preferred over multiple ifs to provide better readability.

Attention! You should always cover ALL possible outcomes. A switch can also have a `default` case to cover all paths that are not covered by the `case` provided.

```c#
int age = 19;
switch(age)
{
  case < 18:
    Console.WriteLine("You are too young to drink in Germany.");
    break;
  case < 21:
    Console.WriteLine("You can drink in Germany but not in the US.");
    break;
  case > 21:
    Console.WriteLine("You can drink.");
    break;
}
```

#### For(each)-Loops

For loops are being used to do the same thing multiple times.

```c#
for (int i = 1; i<=5; i++)
{
    Console.Write(i);
}
//12345
```

For-Each loops are being used to do something for each element of an Array (a group of data)

```c#
string[] names = {"Martin", "Alexander", "Michael", "Jens"};
foreach (string name in names)
{
    Console.WriteLine("Hello "+name);
    // Hello Martin
    // Hello Alexander
    // Hello Michael
    // Hello Jens
}
```

#### While-Loops

There is also a `do { ... } while { ... }` syntax, but the shor tform is way more common.

```c#
int x = 1;
while (x < 100)
{
    Console.WriteLine($"{x} is still smaller than 100, so I will multiply it by 2.");
    x *= 2; // this is the same as x = x * 2;
}
```

### Functions and Methods

Defining and calling functions:

```c#
int Add(int a, int b)
{
    return a+b;
}
int result = Add(3, 4);
Console.WriteLine(result); // 7
```

There are numerous built in methods for the most common operations with basic data types. Here some examples:

```c#
string message = "Hello, World!";

// prints the number of characters in the string: 13
Console.WriteLine(message.Length);

// prints the first 5 Characters: Hello
Console.WriteLine(message.Substring(0, 5));
Console.WriteLine(message.IndexOf("World")); // 7

// 2^4 or 2 * 2 * 2 * 2
double result = Math.Pow(2, 4);
Console.WriteLine(result); // 16

// Takes the square root of the result (16): 4
Console.WriteLine(Math.Sqrt(result));
```

### Object Oriented Programming

#### Classes and Objects

When working with multiple data sets of the same quality you can introduce a class to simplify work. Here an example for humans.

```c#
class Human{
    public string Name { get; set; } // getter and setter are functions being used to get or set a value.
    public int Age { get; set; }

    public void Introduce()
    {
        Console.WriteLine($"Hi! I'm {Name} and I am {Age} years old.");
    }
}

Human john = new Person();
john.Name = "Johnny";
john.Age = 35;
person.Introduce(); // "Hi! I'm Johnny and I am 35 years old."
```

To simplify this even further, you could introduce a 'Constructor' to set values in the same step as initializing the Human.

```c#
class Human{
    private string Name { get; set; }
    private int Age { get; set; }

    public Human(string name, int age)
    {
        Name = name;
        Age = age;
    }
    public void Introduce()
    {
        Console.WriteLine($"Hi! I'm {Name} and I am {Age} years old.");
    }
}

Human john = new Person("Johnny", 35);
person.Introduce(); // "Hi! I'm Johnny and I am 35 years old."
```

#### Inheritance

Sometimes objects are somewhat similar, but yet different. Let's take Animals for example. All of them eat, but only dogs bark. Here the implementation:

```c#
class Animal
{
    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("WUFF!");
    }
}

Animal fish = new Animal();
fish.Eat();

Dog dog = new Dog();
dog.Eat();
dog.Bark();
```

### Collections and Generics

#### Lists

Lists are somewhat similar to arrays but they can shrink and grow dynamically. They also provde built-in functions to use this feature.

```c#
List<string> fruits = new List<string>();
fruits.add("Apple");
fruits.add("Banana");
fruits.add("Cherry");
```

#### Dictionaries / Key-Value

```c#
Dictionary<string, int> agesOfPeople = new Dictionary<string, int>();
ages["Johnny"] = 35;
ages["Michael"] = 17;
ages["Alexander"] = 42;

Console.WriteLine(agesOfPeople["Alexander"]); // 42

// Looping through the dictionary
foreach (KeyValuePair<string, int> kvp in agesOfPeople)
{
    Console.WriteLine($"Name: {kvp.Key}, Age: {kvp.Value}");
}
```

#### Generics

When you want to write a Method in a way that is reusable for multiple types you can do so by using Generic Types.

```c#
ConvertArrayToList<T>(T[] arr)
{
    List<T> list = new List<T>();
    foreach (T item in arr)
    {
        list.add(item);
    }
    return list;
}
string[] names = {"Martin", "Alexander", "Michael", "Jens"};

List<string> listNames = ConvertArrayToList(names);
```

## 📚 Conclusion

C# is a powerful and feature rich programming language. Together with the [nuget](https://www.nuget.org/packages) package manager you can use pre-built feature from other users or create your own! Either way it is recommended to learn a language by speaking it. So start your own project.

## 🚧 Starter Projects

It is recommended to start with a simple and small project that can be finished within a short amount of time. Common projects to start are:

- Todo App
- Calculator
- Tic-Tac-Toe
- Rock Paper Scissors
- Pinball
- Snake

When you feel comfortable with C# and feel like you want to do more: Check out the .NET Framework to build Webapplications or APIs.

## ⁉ FAQ

### What IDE should i use for C# Development

To start out Visual Studio is your best bet. If you get more serious you can look into a Jetbrains subscription for [Rider](https://www.jetbrains.com/rider/).

Working with MacOS will become harder in August 2024, when Visual Studio is discontinued. Microsoft recommends Visual Studio Code with some Plug Ins, but keep in mind Rider is a valid option as well if you want to afford it.

### How to run and debug my C# programs

Visual Studio and Rider both come with a set of robust debugging tools. Set breakpoints and run your app, to see and analyze the state of your program in any given moment. This is also a nice way to learn and understand more about the internals of C#.

### What is the best way to learn C#

Practice! Set yourself a goal of what you want to achieve. For example an Inventory App that tracks the stock of your company. Work until you get there and if you are stuck it is time to read some documentation or in depth guide for the feature you need to learn.

Collaborate with other people or teams. Join hackathons or some open source project. You can learn from others and their code.

Check the patch notes whenever there is a new version of C# coming your way. This way you know about the new features - try to use them in your code and refactor old code to stay up to date!

## 🙏🏽 Thanks

Thank you so much if you read this article all the way! Leave a comment if you have any questions, I'll be more than happy to answer right away. If you are shy you can also message me directly on [GitHub](https://github.com/Suneeh), [Instagram](https://www.instagram.com/_suneeh/) or [TikTok](https://www.tiktok.com/@_suneeh).

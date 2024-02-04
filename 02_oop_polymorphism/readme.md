<style>
p {
  max-width: 992px;
}
pre {
  max-width: 992px;
}
h1 {
  font-size: 3rem;
}
h2 {
  font-size: 2.6rem;
}
h3 {
  font-size: 2.2rem;
}
h4 {
  font-size: 1.8rem;
}
h5 {
  font-size: 1.6rem;
}
</style>  

# Advanced Object-Oriented Programming Concepts

Welcome to the Advanced Object-Oriented Programming in Python module! This
module is designed to take your understanding of Object-Oriented Programming
(OOP) to the next level, focusing on more sophisticated aspects of working with
classes and objects. You will explore advanced features of Python that enable
powerful and flexible software design patterns. Topics such as the `@property`
decorator for implementing getters, setters, and property management, along with
polymorphism, method overriding, and inheritance, are covered in detail. 

## Module Topics:
1. **The @property Decorator**:
   - Dive into the `@property` decorator to understand its role in defining
     class properties that act like attributes. 
   - Learn how to use getters and setters to manage object state in a controlled
     manner, adhering to the principles of encapsulation. 
   - Explore the concept of caching with `@cached_property` to optimize
     performance for computationally expensive operations. 

2. **Polymorphism in Python**:
   - Understand polymorphism and how it allows for flexibility in interface
     design, letting different objects be used interchangeably. 
   - Discover how polymorphism can be implemented through method overriding and
     duck typing, enabling objects of different types to be processed with
     common interface. 

3. **Method Overriding and `super()`**:
   - Learn the nuances of method overriding to alter or extend the behavior of
     inherited methods in subclasses. 
   - Use the `super()` function to call methods from a parent class within an
     overridden method, providing a way to extend rather than replace
     functionality. 

4. **Inheritance in Depth**:
   - Deepen your understanding of inheritance, a cornerstone of OOP that allows
     classes to inherit attributes and methods from other classes. 
   - Explore single inheritance, multiple inheritance, and the diamond problem,
     along with best practices for designing and using class hierarchies. 

5. **Functional Overriding**:
   - Examine how Python supports functional overriding to modify or extend the
     behavior of functions and methods, providing dynamic flexibility in object
     behavior. 

Throughout this module, practical exercises and programming challenges will help
you apply these concepts in real-world scenarios. You will get hands-on
experience with advanced OOP features, enhancing your ability to design and
implement sophisticated software systems. 

This journey will equip you with the knowledge to use advanced OOP features
effectively, enabling you to write cleaner, more efficient, and reusable code.
By mastering these concepts, you'll be well-prepared to tackle complex
programming challenges and create versatile Python applications. 

Embark on this advanced exploration of object-oriented programming in Python,
and unlock new possibilities in your programming projects! 

## Notebooks

*Review the basic concepts of object programming in the [notebooks in the review folder](./learn/review/)*

[Property Decorator](./learn/01_property_decorator.ipynb)

[Duck Typing and EAFP](./learn/02_duck_typing_eafp.md)

[Function Overloading](./learn/03_function_overloading.md)

[Inheritance](./learn/04_inheritance.ipynb)

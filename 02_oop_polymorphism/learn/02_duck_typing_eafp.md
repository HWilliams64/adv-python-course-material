# Duck Typing and EAFP in Python

## Duck Typing

<style>
html,body        {height: 100%;}
.wrapper         {width: 80%; max-width: 1280px; height: 100%; margin: 0 auto; background: rgba(255, 255, 255, .0); padding-bottom: 50px}
.h_iframe        {position: relative; padding-top: 56%;}
.h_iframe iframe {position: absolute; top: 0; left: 0; width: 100%; height: 100%;}
</style>

<div class="wrapper">
    <div class="h_iframe">
        <iframe height="2" width="2" src="https://www.youtube.com/embed/iapiDqf_IYI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
    </div>
</div>

Duck Typing is a concept in Python that focuses on what an object can do, rather than what an object is. This means that the type or class of an object is less important than the methods or attributes it defines. The name comes from the phrase "If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck."

### Example of Duck Typing

```python
class Duck:
    def quack(self):
        print("Quack, quack!")

class Person:
    def quack(self):
        print("I'm quacking like a duck!")

def make_it_quack(duck):
    duck.quack()

duck = Duck()
person = Person()

make_it_quack(duck)  # Output: Quack, quack!
make_it_quack(person)  # Output: I'm quacking like a duck!
```

- **The `make_it_quack` function**: It expects an object that has a `quack` method, regardless of the object's type.
- **Duck and Person classes**: Both have a `quack` method. Despite their different types, they can both be passed to `make_it_quack`.

## Easier to Ask Forgiveness than Permission (EAFP)

EAFP is a programming principle that recommends trying to do something that might fail and catching exceptions if it does, rather than checking in advance if it will succeed. This is in contrast to the Look Before You Leap (LBYL) approach, where you check conditions before making the call.

### Example of EAFP

<style>
html,body        {height: 100%;}
.wrapper         {width: 80%; max-width: 1280px; height: 100%; margin: 0 auto; background: rgba(255, 255, 255, .0); padding-bottom: 50px}
.h_iframe        {position: relative; padding-top: 56%;}
.h_iframe iframe {position: absolute; top: 0; left: 0; width: 100%; height: 100%;}
</style>

<div class="wrapper">
    <div class="h_iframe">
        <iframe height="2" width="2" src="https://www.youtube.com/embed/Mp1XL7boXfg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
    </div>
</div>


```python
def read_file(file_name):
    try:
        with open(file_name) as file:
            print(file.read())
    except FileNotFoundError:
        print("The file does not exist")

read_file("example.txt")  # Tries to read example.txt and handles the exception if the file is not found.
```

- **Try-Except Block**: Attempts to open and read the file, catching the `FileNotFoundError` exception if the file does not exist.
- **EAFP in Python**: Encourages writing cleaner and more readable code by handling exceptions for unexpected conditions.

### How EAFP Relates to Polymorphism

EAFP and Duck Typing both promote flexibility in Python's approach to polymorphism. Duck Typing allows objects to be used polymorphically based on their capabilities rather than their types, and EAFP supports a more dynamic and flexible way of handling errors through polymorphic exception handling. Together, they enable developers to write more generic and reusable code that can work with a wide range of object types and structures, enhancing polymorphism.

- **Flexibility and Reusability**: Using EAFP and Duck Typing together allows for more adaptable and robust code that can handle a variety of situations dynamically.
- **Polymorphic Behavior**: Both principles support the use of objects in polymorphic ways, based on their behavior (Duck Typing) or through flexible error handling (EAFP), without being constrained by their specific types.
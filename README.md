Object orientation in Python
===

In this tutorial we are going to play around with classes and objects in Python.
We will create a class with several methods and attributes, analyze it carefully and use it to create a couple of objects. We will also see a simple example of inheritance.
To write Python commands we will use the plain Python interpreter.


Getting started
---
In this tutorial will use Python 2.7, but if you have any other version it is also fine. The Python interpreter we will be using comes with the installer. If you already have Python on your machine you can skip the instructions below.
More experienced users might also want to work with [IPython](http://ipython.org/install.html), an interactive console which offers additional functionality such as tab completion, inspection of the namespace, inline help and much more. 


For **Windows** users the most convenient way to get Python is to download the installer from the official [Python website](https://www.python.org/download/). Depending on your operating system type, you will need either a 32-bit or 64-bit version of the installer.
If you're unsure which version to download, you can find the information by right clicking on `My Computer` and `Properties` in Windows. If you have a 32-bit processor get the `Python 2.7.6 Windows Installer` and if it's 64-bit you need the `Python 2.7.6 Windows X86-64 Installer`.

**Mac** and **Linux** users should already have Python on their systems, since it comes with the operating system. 
Experienced users might wish to check out which version of Python they have by starting a Terminal and typing in: 
```python --version```
If you want to update or install a newer Python version (recommended only for users with Python 2.5) on a Linux system just start a Terminal and type:
```
sudo apt-get install python
```
Mac users should also use the Terminal to first install `homebrew` which is then used to download a new version of Python. To do so, type in the following commands:
```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
brew install python
```

The first class
---
Let's start by creating a simple book class:
```python
class Book(object):
	def __init__(self):
		self.title = "1984"
		self.author = "George Orwell"
	
	def print_info(self):
		print "The book", self.title, "is written by", self.author

```

Well, we see that we have two methods: `__init__` which is a constructor and a `print_info` method.
They both take just one argument, namely the `self` which is used to address attributes and methods.
In the constructor we define two attributes: `title` and `author`, and set them to some predefined values.
In the method `print_info` we print these values. Huh? This doesn't appear to be particularly exciting, let's actually use this class. First, we create an object, namely an instance of a class `Book`:

```python
book1 = Book()
```

Now we can access class members, let's see what happens when you type:
```python
book1.title
book1.author
book1.print_info()
```
Nothing unexpected, right? We see that the class is holding a state of an object, defined through its attributes `title` and `author`.
Now, let's create another instance of the class `Book` and print some information about it:
```python
book2 = Book()
book2.print_info()
```
Ok, it looks pretty much like the `book1`. But if we change the book title, or even add new attributes in the `book1`, what happens with the `book2`, can you guess?

```python
# Edit the value of an existing attribute
book1.title = "Animal Farm"

# And create a new attribute
book1.year = 1945

# What happens here?
book1.print_info()
book2.print_info()

```

If your answer was "Nothing changed", you were right! 
This is because `book1` and `book2` are just instances of a class, and instance attributes are not shared among each other. If we want to take a peek into the class members, we can use the function `dir`:
```python
dir(book1)
```
The output should be following:
```
['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'author', 'print_info', 'title']
```
At the end of the output we see the class members we defined - attributes `author` and `title`, and the `print_info` method. Other strange looking class members is something Python adds by defult, but we won't use them in this exercise. 

Now, let's make a slightly different class than the one before, and create a corresponding object:
```python
class Book(object):
	def __init__(self):
		title = "1984"
		author = "George Orwell"
	
	def print_info(self):
		print "The book", title, "is written by", author

book = Book()
```
What happens if you try to access variables `title` and `author?` 

You should get a warning saying that `Book` has no attribute 'title'. This might be a bit confusing, as we defined these two variables in the `__init__` method. But, since we didn't make them attributes (notice the `self` is missing), Python treats them as local variables which exist only in the scope of the function and get destroyed after the function is executed. Check out that these attributes don't exist using the `dir` command.

Customizing a class
---
In the previous example, we set attribute values in the constructor and changed them later by hand.
We can also save us some work and pass the book title and the author name as arguments to the constructor.
To do so, we extend the list of arguments in the constructor. 

```python
class Book(object):
	
	def __init__(self, title, author):
		self.title = title
		self.author = author
		
	def print_info(self):
		print "The book", self.title, "is written by", self.author
		
```

Now, we create an instance in a following way:
```python
book1 = Book("Oscar Wilde", "The Picture of Dorian Gray")
book1.print_info()
```

Oh crap, we switched the title and the author! Do you have any idea how to fix that? 
In case you forget the order of your arguments, you can always use the following syntax:
```python
book1 = Book(author="Oscar Wilde", title="The Picture of Dorian Gray")
book1.print_info()
```

## Inheritance 
In this exercise, we're going to explore some of the advantages of using inheritance, an important aspect of object-oriented programming. Remember, inheritance means we define a class and inherit its members in another class. The class we're inheriting from is called a base class, and the class which inherits is the derived class.
Let's start with a class which is very similar to the `Book` class from before:
```python
class Book(object):
	
	def __init__(self, title, author, nr_pages):
		self.title = title
		self.author = author
		self.nr_pages = nr_pages
		
	def print_info(self):
		print "The book", self.title, "is written by", self.author, "and contains", self.nr_pages, "pages"
		
	def repair_costs(self):
		return self.nr_pages * 2

```
Now, let's create another class `SpecialBook` which inherits from the class `Book`:
```python
class SpecialBook(Book):
	pass
```
Now, `pass` is used to denote that we don't do anything new in the class, we just inherit all the members from the class Book. We are not adding or changing any behaviour of our new class. Basically, `SpecialBook` and `Book` are two different classes that behave in the same way.
You can convice yourself by creating two instances and comparing them:
```python
book1 = Book("Mrs Dalloway", "Virgina Woolf", 100)
book2 = SpecialBook("Emma", "Jane Austen", 100)
book2.print_info()
book2.repair_costs()
```
As you can see, even if we didn't define any of the methods in the `SpecialBook` class, they are there because we inherited them from the parent class. Now, this is nice, but let's see a more practical example. 
Let's say that all special books are more expensive to repair, we can define a method in `SpecialBook` and keep other attributes same as before:
```python
class SpecialBook(Book):
	def repair_costs(self):
		return self.nr_pages * 5
```

Let's see what changes now:
```python
book1 = Book("Mrs Dalloway", "Virgina Woolf", 100)
book2 = SpecialBook("Emma", "Jane Austen", 100)
book2.print_info()
book2.repair_costs()
```

We see that the special book has much higher cost now, and this is because we used an OOP feature called overriding. Overriding means we changed the method inherited from a parent class, but kept its name.


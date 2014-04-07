Object orientation in Python
===

In this tutorial we are going to play around with classes and objects in Python.
We will create a class with several methods and attributes, analyze it carefully and see important differences between class attributes and instance attributes. 
To write Python commands we will use the plain Python Interpreter.


Getting started
---
In this tutorial will use Python 2.7, although if you have any other version it is also fine. The Python Interpreter we will be using comes with the installer. If you don't have Python on your machine follow the steps below, otherwise you can skip this section.
More experienced users might also want to work with [IPython](http://ipython.org/install.html), an interactive console, which offers additional functionality such as tab completion, inspection of the namespace, inline help and much more. 


For **Windows** users the most convenient way to get Python is to download installers from the official [Python website](https://www.python.org/download/). Depending on your operating system type, you will need either a 32-bit or 64-bit version of the installer.
If you're unsure which version to download, you can find the information by right clicking on `My Computer` and `Properties` in Windows. If you have a 32-bit processor get the `Python 2.7.6 Windows Installer` and if it's 64-bit the `Python 2.7.6 Windows X86-64 Installer`.

**Mac** and **Linux** users should have Python already installed by default. 
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
They both take just one argument, namely the `self` which says that these two methods belong to exactly that class.
In the constructor we define two attributes: `title` and `author`, and set them to some predefined values.
In the method `print_info` we print these values. Huh? That looks rather boring, let's actually use this class.

```python
# First, we create one instance of a class Book
book1 = Book()
```

We can access class members, let's see what happens when you type:
```python
book1.title
book1.author
book1.print_info()
```
Nothing unexpected, right? 
We see that the class is holding a state of an object, defined through its attributes `title` and `author`.
Now, let's create another instance of a class `Book` and print info about it:
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
This is because `book1` and `book2` are just instances of a class, and instance attributes are not shared among each other.

Customizing a class
---
In the previous example, we set default values for a book and changed them by hand later.
We can also save us some work and pass the book title and the author name upon the creation of an instance.
To do so, we extend the list of arguments in the constructor. 
In addition, we added a new attribute, a list called `borrowed_to` which helps us keep the track of persons whom we borrowed our book to. 
For track keeping we create a new method, that takes the name as an argument and appends it to the list.

```python
class Book(object):
	
	borrowed_by = []
	
	def __init__(self, title, author):
		self.title = title
		self.author = author
		
	def print_info(self):
		print "The book", self.title, "is written by", self.author
		
	def borrows(self, name):
		self.borrowed_by.append(name)
```

Now, we create an instance in a following way:
```python
book1 = Book("Oscar Wilde", "The Picture of Dorian Gray")
book1.print_info()
```

Oh crap, we switched the title and the author! Do you have any idea how to repair that? 
In case you forget the order of your arguments, you can always use the following syntax:
```python
book1 = Book(author="Oscar Wilde", title="The Picture of Dorian Gray")
book1.print_info()
```
This looks much better! Now, let's expand our library by another book:
```python
book2 = Book(author="Oscar Wilde", title="The Importance of Being Earnest")
```

Suppose you want to lend the first book to Ann, and the second book to Joe:
```python
book1.borrows("Ann")
book2.borrows("Joe")
```
Ok, then we forget whom we gave our books, so we want to remind ourselves:
```python
book1.borrowed_by
book2.borrowed_by
```
Wait, the list says that both books have been with Ann and Joe, which is not true.
What happened here? By defining a list `borrowed_by` out of the scope of any method, 
we declared it as class attribute which is shared among all instances. 
Thus, when one instance appends the value, this is executed in all instances.


Mind-teasers
----
1. Suppose you made a very common typo and wrote a class like this:
  ```python
  class Book(object):
  	
  	borrowed_by = []
  	
  	def __init__(self, title, author):
  		self.title = title
  		self.author = author
  		
  	def print_info():
  		print "The book", self.title, "is written by", self.author
  		
  	def borrows(self, name):
  		self.borrowed_by.append(name)
  ```
  
  Now, Python assumes you know what you're doing and doesn't complain. But then when you wanted to print the information about the book, you get a following message:
  
  ```python
  >>> book1.print_info()
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  TypeError: print_info() takes no arguments (1 given)
  ```
  Can you guess what is that mysterious argument that has been given to a function?

2. If instead of `borrowed_by` we had kept track of a number of times we read a book, like this:
  ```python
  class Book(object):
  	
  	times_read = 0
  	
  	def __init__(self, title, author):
  		self.title = title
  		self.author = author
  		
  	def print_info():
  		print "The book", self.title, "is written by", self.author
  		
  	def read(self, name):
  		self.times_read = self.times_read + 1
  
  book1 = Book(author="Oscar Wilde", title="The Picture of Dorian Gray")
  book2 = Book(author="Oscar Wilde", title="The Importance of Being Earnest")
  
  book1.read()
  book2.read()
  book1.read()
  ```
  
  What would you expect `book1.times_read` and `book2.times_read` to be? Check your answer by trying it out. 
  For a detailed explanation, check [this](http://stackoverflow.com/questions/2923579/python-class-attribute) answer on Stack Overflow.

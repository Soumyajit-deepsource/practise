This is Kenshoo's Python Style Guide. Using this style guide we hope to acheive the following:

Humbly share our knowledge & expeience with the world (and some of our pain too).
Assist novice/intermediate developers.
Generate healthy discussion which will benefit all.
Admit and accept we may be wrong and change per the community's comments.
We DO NOT wish the following:

Say/claim/Imply there is a "right" or "wrong" way to do it.
Claim this style guide is inclusive.
Introduce ourselves as some kind of language "experts".
Inspired by many style guides but mainly from Airbnb's Style-Guide "style" :-).

Why not just read PEP 008?
You SHOULD read python's PEP 008 as it contains the basic foundations for writing Python code.

This Style Guide aims to share our experience of writing with Python conisdering syle guides such as PEP 008. It therefore reflects our own conclusions and preferences.

Table of Contents
PEP 20
General
Naming
Code Layout
Indentation
White Spaces
New Lines
Conditional Expressions
Flow
Truthy vs Falsey
Single Line
Ternary
Forgiveness vs Permission
Strings
Double vs Single Quotes
Concatenation
Methods, Functions, and Callables
Arguments/Parameters
Parameter Passing
Dynamic Parameter Expansion
Lambdas and Nested Functions
Recursion
Collections/Iterables
Slice
Comprehensions
Builtin Functions For Collections
Tuples
Tuple vs List
Imports
Classes
Class Methods
Exceptions
Catching Exceptions
EAFP
Custom Exceptions
Regular Expressions
PEP 20
The Zen Of Python

General
Don't explain code with comments
Instead of a writing a comment explaining the code, place the code into a function whose name explains your intent.

Naming
PEP 8 naming convention

In general:

Use snake_case for modules, methods and variables.
Use CamelCase for classes. (Keep acronyms like HTTP, RFC, XML uppercase.)
Use SCREAMING_SNAKE_CASE for other constants.
The names of predicate methods (methods that return a boolean value) should start with is, does, has, or the likes.
The name of predicates should also be positive. (i.e. is_alive, is_empty versus is_not_dead, is_not_empty)
Generators (especially comprehension generators), iterators, and other lazy loading objects names should not imply the underlying implementation, but rather the result you expect.
# bad
generate_screaming_list = (word.upper() for word in list_of_words)

for word in generate_screaming_list:
    print(word)

# good
screaming_list = (word.upper() for word in list_of_words)

for word in screaming_list:
    print(word)
Throwaway variables should be named with an underscore (_).
If you feel the need to describe the variable, then name the variable using a leading underscore.
for _ in range(4):
    print 'hi'

for _number in range(4):
    print('hi')
Code Layout
Indentation
Use soft-tabs with a four space-indent
Align function parameters either all on the same line or one per line.
# bad
def create_translation(phrase_id, phrase_key, target_locale,
                        value, user_id, do_xss_check, allow_verification)
    ...

# good
def create_translation(phrase_id,
                        phrase_key,
                        target_locale,
                        value,
                        user_id,
                        do_xss_check,
                        allow_verification)
    ...

# good
def create_translation(
    phrase_id,
    phrase_key,
    target_locale,
    value,
    user_id,
    do_xss_check,
    allow_verification
)
    ...
White Spaces
Avoid extraneous whitespaces within parentehses, brackets, and braces.
Preferred not to use multiple white spaces during assignment
# bad
long_name = 'str'
a         = 9
b         = 8

# good
long_name = 'str'
a = 9
b = 8
Keyword arguments and default values should not contain whitespaces
New Lines
There should be two newlines after all your import statements.
Don’t include newlines between areas of different indentation (such as around class or method bodies).
# bad
class Foo(object):

    def bar(self):
        # body omitted

# good
class Foo(object):
    def bar(self):
        # body omitted
Use a single empty line to break between statements to break up methods into logical paragraphs internally.
def transformorize(self, _car):
  car = manufacture(options)
  t = transformer(robot, disguise)

  car.after_market_mod()
  t.transform(car)
  car.assign_cool_name()

  fleet.add(car)
  return car
End each file with a newline. Don't include multiple newlines at the end of a file.
Conditional Expressions
Prefer to check positive statements vs negative statements
Flow
if/else blocks should handle the least amount of logic first
# bad
if reason:
    x = get_value()
    y = x * 10
    # code
    # code
    # code
    return x
else:
    return 10

# good
if not reason:
    return 10
else:
    x = get_value()
    y = x * 10
    # code
    # code
    # code
    return x
So too, single line jumps/flow control disruption should not be handled in an else scope
# bad
for i in items:
    if condition:
        # code
        # code
    else:
        break /return

# good
for i in items:
    if condition:
        break / return

    # code
    # code

# bad
if condition:
    # code
    # code
else:
    raise Exception

# good
if condition:
    raise Exception

# code
# code
Avoid nesting if statements.
If nesting is needed never more than 2 levels of nesting.
Place and / or keywords at the end lines when using a line break.
# bad
if (name == 'bob'
    and l_name == 'samuels'):
    ...

# good
if (name == 'bob' and
    l_name == 'samuels'):
    ...
Truthy vs Falsey
Prefer truthy/falsey checks vs comparing actual values.
"truthy" values are all objects except

False
None
0
[] empty lists
() empty tuples
{} empty dictionaries
an object whose magic method __bool__ returns falsey(in python 3)
an object whose magic method __nonzero__ returns falsey(in python 2)
# bad
if value > 0:
    ...

# good
if value:
    ...

# bad
if value is 0:
    ...

# good
if not value:
    ...

# bad
if len(a_list) > 0:
    ...

# good
if a_list:
    ...

# bad sometimes
val = None
if val is None:
    ...

# good
val = None
if not val:
    ...
If you need to check an object if it is None and not falsey use the is operator
# bad
if val == None:
    ...

# good
if val is None:
    ...
Single Line
You can use single line if statements when they are short and simple (like checking truthy statements).
Don't use single lines when you using combined conditions.
# bad
if complex_object.with_a_long_name.some_value > complex_object.target_value: return complex_object.with_a_long_name.some_value

# bad
if condition and another_condition: return value

# good
if satisfied: return tip

# Acceptable switch case like conditioning
if very_happy: return tip * 1.5
if angry: return lip
return tip
Ternary
Avoid the ternary statement except in cases where all expressions are extremely trivial.
Avoid multiple conditions in ternaries.
Use the ternary operator over if/else/ constructs for single line conditionals.
# bad
return val if condition and other_condition else other_val

# bad
if some_condition:
    return val
else:
    return other_val

# good
return val if some_condition else other_val
Do not nest ternary expressions.
Prefer if/else constructs in those cases.

# sinful
x = val if some_condition else other_val if nested_condition else something_else

# good
if some_condition:
    x = val
else:
    x = other_val if nested_condition else something_else
Forgiveness vs Permission
It is 'Easier to ask for forgiveness than permission'.

When writing code that may throw an exception do not check for that possibility.
Assume it will work and catch the exception.
Make sure to catch the expected exception and not all exceptions.
# bad
if some_dict.has_key('a_key'):
    do_something(some_dict['a_key'])
else:
    ...

# good
try:
    do_something(some_dict['a_key'])
exception KeyError:
    ...

# bad
if val != 0:
    10 / val

# good
try:
   10 / val
except ZeroDivisionError:
    ...
Strings
Do not compare strings with is.
is is inconsistent when comparing strings.

# bad
name is 'bob'

# good
name == 'bob'
It is acceptable to compare with is when a global object is expected.
BOB = 'bob'

def get_name():
    return BOB

name = get_name()

# acceptable
if name is BOB:
    ...
Double vs single Quotes
Prefer double quotes when your string has multiple words or when formatting ("hi bob").
If the string is a single word use single quotes ('bob').
Concatenation
Prefer string formatting over string concatenation.
When formatting prefer fstrings over str#format (f"hi {name}")
Besides for being cleaner this optimizes performance (less work for GC)

# bad
email_with_name = user.name + ' <' + user.email + '>'

# good
email_with_name = "{} <{}>".format(user.name, user.email)

# better
email_with_name = "{name} <{mail}>".format(name=user.name, mail=user.email)

# best
email_with_name = f"{user.name} <{user.mail}>"
Avoid appending strings (+=) when you need to construct large data chunks. Instead, use .join.
# bad
statement = ''
for i in a_list:
    statement += i

# good
''.join(i for in a_list)
Methods/Functions/Callables
See class functions as well.

Checkout Builtin Functions For Collections as well.

A callable's body should be no more than four statements.
Predicate methods are encouraged.
Arguments/Parameters
Your functions should have up to 3 parameters (not including self and kind).
Use packing syntax (*args, **kwargs) in method declarations to reduce arguments.
When packing arguments, pack those who are most related to each other and provide a meaningful name.
# Bad
def post_answer(**kwargs):
    requests.post(kwargs['host'] + kwargs['path'], json=kwargs['body'])

# Still not good
def post_answer(host, path, **kwargs):
    requests.post(host + path, json=kwargs)

# Good
def post_answer(host, path, **body):
    requests.post(host + path, json=body)
Parameters that are only received to pass to other functions should be reduced.
If you see a parameter untouched being passed to a few functions, try to put that data into an object or closure who will be passed instead.

    # Bad
    def funk1(container, key, erase=True):
        # Do some other actions
        funk2("value", container, key, erase)

    def funk2(funk2_param, container, key, erase=True):
        # Do some other actions
        funk3(container, key, erase)

    def funk3(container, key, erase):
        if erase:
            try:
                del container[key]
            except KeyError:
                print "no key:{} to erase".format(key)


    funk1(container, key)

    # Good
    def funk1(container, key, erase=True):
        # Do some other actions
        def erase_later():
            if erase: del container[key]
        funk2("value", erase_later)


    def funk2(funk2_param, action):
        # Do some other actions
        funk3(action)


    def funk3(action):
        try:
            action()
        except KeyError:
            print "no key:{} to erase".format(key)


    funk1(container, key)

    # Also good
    def funk1(container, key, erase=True):
        # Do some other actions
        funk2("value", container=container, key=key, erase=erase)

    def funk2(funk2_param, **funk3_params):
        # Do some other actions
        funk3(**funk3_params)

    def funk3(container, key, erase):
        if erase:
            try:
                del container[key]
            except KeyError:
                print "no key:{} to erase".format(key)


    funk1(container, key)
Default arguments are encouraged, but DO NOT use mutable objects.
Default arguments are evaluated once only during module load time.

So the same original object will be constantly used and modified, when you probably intended a new object.

def action(a_list=[]):
    a_list.append(1)
    return a_list

action() # => [1]
action() # => returned [1,1] when you expected [1]
Do not use a statement/expression (something to be evaluated) within an argument list
# Very bad
def funktion(a, b=mod.get_list()):
  pass
Parameter Passing
It is best to be explicit when passing arguments.
Especially boolean options.
This adds readability.

def save_file(file_name, log):
    if log:
        print("Saving file {}".format(file_name))
    ...
# Very Bad
save_file("myfile.txt", True)

# Good
save_file("myfile.txt", log=True)

# Better
save_file(file_name="myfile.txt", log=True)
Dynamic Parameter Expansion
You can pass a dynamic list of parameters by using unpacking syntax.
func_call(*args, **kwargs)

Lambdas and Nested Functions
We encourage using closures to create partial applications and the like.
This is especially true when used to add readability to comprehension statements

# bad
users_from_nyc = [create_user(user, 'nyc', paid=True)
                  for user in users]

# good
create_user_from_nyc = lambda user: create_user(user, 'nyc', paid=True)
users_from_nyc = [create_user_from_nyc(user) for user in users]
Use lambdas for one liners.
If a lambda expression is long (90+ chars) use a nested function.
Prefer existing standard library functions over lambdas.
# bad
my_multiply = lambda a,b: a * b
my_multiply(5,5)

# good
from operator import mul
mul(5,5)
Recursion
Avoid recursion.
The for loop is, effectively, the same abstraction that recursion provides in functional programming languages.

Collections/Iterables
There are three major skills to apply to collections.

slicing, comprehensions, and utilizing builtin functions.

The purpose of this writing is to automatically associate our collection solutions with these three tools.

Pushing us away from using code as such:

    # Bad
    my_new_list = []

    for i in old_list:
        if some_check(i):
            my_new_list.append(i)
Slice
slicing helps a lot, especially with strings.

    # Slice structure
    items[start:end:step]
A slice can have up to three attributes.

start - our starting index

end - the index in which to stop (not including)

step - the stepping process

step is interesting because it controls how to jump between items including direction.

i.e.

    items[::-1] # list reversed
    range(100)[2::2] # return all even numbers
    items[4::-1] # starting at 5th position go backwards

    def palindrome_check(s):
        midway, is_odd = divmod(len(s), 2)

        return s[midway - 1::-1] == s[midway + is_odd:]

    palindrome_check('hello olleh') # => True
    palindrome_check('noon') # => True
    palindrome_check('joe') # => False
Using Slice
Use implicit values
# bad
end_of_list = len(a_list)
a_list[2:end_of_list]

# good
a_list[2:]

# bad
a_list[0:5]

# good
a_list[:5]

# bad
reversed = a_list[0:end_of_list:-1]

# good
reversed = a_list[::-1]
Do not compromise readability when using slice
Consider using slice constructor

numbers = range(100)

# bad
numbers[2::2] # => all even numbers starting from 2

# good
even_numbers = slice(2, None, 2)

numbers[even_numbers] # => all even numbers starting from 2
Comprehensions
list comprehension
Syntax is:

[i for i in numbers]
Prefer to use list comprehension when mapping
# bad
result = []
for i in numbers:
    result.append(i * 2)

# bad
result = map(lambda i: i * 2, numbers)

# good
result = [i * 2 for i in numbers]
Prefer to filter with list comprehension
# bad
odd_numbers = []
for i in numbers:
    if i % 2 == 1:
      odd_numbers.append(i)

# not preferred
is_odd = lambda i: i % 2 == 1
odd_numbers = filter(is_odd, numbers)

# good
is_odd = lambda i: i % 2 == 1
odd_numbers = [i for i in numbers if is_odd(i)]
Keep logic out of comprehension statements (use callables)
# bad
odd_numbers = [i for i in numbers if i % 2 == 1]

# good
is_odd = lambda i: i % 2 == 1
odd_numbers = [i for i in numbers if is_odd(i)]
Generator Statements aka lazy iteration
syntax is:

(i for i in a_list)
Generator statements allows filtering, chaining, and all the same results list comprehension can accomplish.

However, generators are lazy. Meaning they only evaluate and return a value upon call.

Always prefer generator statements over temporary list construction
Many times we construct lists just to produce another list in order to use that list.

Generators reduce how many times we iterate over the same object.

groups = [
    ["sara", "sally", "bob"],
    ["todd", "gary", "jon"],
    ["sue", "jerry", "bob"]
]

def invite_bobs_friends(people):
    for person in people:
        invite(person)

# bad
people_who_know_bob = [person
                       for group in groups
                       if "bob" in group
                       for person in group]

people_who_know_bob_excluding_bob = [person for person in people_who_know_bob if person != 'bob']

invite_bobs_friends(people_who_know_bob_excluding_bob)

# good
people_who_know_bob = (person
                       for group in groups
                       if "bob" in group
                       for person in group)


people_who_know_bob_excluding_bob = (person for person in people_who_know_bob if person != 'bob')

invite_bobs_friends(people_who_know_bob_excluding_bob)
Chaining Iterations
Use new lines to maintain readability
Avoid using over 2 statements within list comprehension
groups = [
    ["sara", "sally", "bob"],
    ["todd", "gary", "jon"],
    ["sue", "jerry", "bob"]
]

# still bad
people = []
for group in groups:
    for person in group:
        people.append(person)

# good
people = [person for group in groups for person in group]

# better
people = [person for group in groups
          for person in group]

# hard to follow
people_who_know_bob = [person for group in groups if "bob" in group for person in group]

# better but still hard to follow
people_who_know_bob = [person
                       for group in groups
                       if "bob" in group
                       for person in group]
Consider using generators to maintain readability
groups = [
    ["sara", "sally", "bob"],
    ["todd", "gary", "jon"],
    ["sue", "jerry", "bob"]
]

# best
groups_with_bob = (group for group in groups if "bob" in group)

people_who_know_bob = [person for person in groups_with_bob]
Builtin Functions For Collections
There are builtin functions that satisfy most use cases with collections (lists, tuples, dicts, sets, etc).

Actions on collections like sorting, value checking, and creating new unique lists can accomplished with builtins.

However, Python's for loops where optimized for high performance (comprehension even more so).

Python is not optimized for function calling and recursion.

This means a regular for loop may out perform a recursive solution.

Considering this, we prefer loops over functional methods like map and filter.

This rule is not iron clad.

Functional programming is greatly beneficial from a design standpoint.

There are many third party libraries dedicated to functional programming.

There are also many builtins that have the best of both, becasue they can accept comprehension statements.

Here is a list of useful builtin functions:

all - checks if all values are truthy
any - checks if any value is truthy
all and any check if all or any values in an iterable are truthy

    all([1,2,0]) # => False
    all([1,2,5]) # => True

    any([0,9,0]) # => True
    any([])  # => False

    # Very useful with comprehension statement
    any(i for i in numbers if is_even(i))
Be careful when using all
All returns True when given an empty list or a generator that yields nothing.

numbers_over_4 = (i for i in range(4) if i > 6) # => yields no values
all(numbers_over_4) # => True
When asserting values prefer not to use all. Instead use regular for loop.
assert all hides which value failed.

# sexy but bad
assert all(value for value in a_list)

# good
for value in a_list:
    assert value

# better
for value in a_list:
    assert value, "bonus descriptive message about {}".format(value)
enumerate - iterate with accompanying climbing value
for index, name in enumerate(['bob', 'joe', 'sam']):
    print 'index:', index
    print 'name:', name
filter - filters a list
In python3 filter produces an iterator

filter(lambda i: i % 2, range(10))
map - creates a list based on values of another list
There is no reason to use map unless you want to map two iterables with a function that would accept two parameters (one from each iterable).

Even then you can probably get the same result using zip.

# bad
map(lambda i: i * 10), numbers)

# prefer
[i * 10 for i in numbers]

# acceptable
# apply two lists to function that takes two parameters
add = lambda i,j: i + j
map(add, numbers1, numbers2)

# consider
[sum(values) for values in zip(numbers1, numbers2)]
max | min - gets max or min value of list
sum - adds the values (should be int) of list
reduce - with logic creates a single value from list values
set - distinct list
sorted - sorts values of collection
in - finds item in collection (in is a operator)
Look at the documentation Python3 Builtins

Most of the these builtin functions and others accept a comprehension statement.

Remember comprehension in python is optimized.

Add on that the ability to filter and map.

Therefore, using comprehension syntax is always preferred.

' '.join(word for word in speech if allowed(word))

any(word for word in speech if allowed(word))
For more tools used for manipulating iterables checkout Itertools

Tuples
Don't use implied tuples
It isn't clear that implied tuples are tuples, so avoid them.

Unless it's part of a DSL or multiple instantiation where our intentions are clear.

# Bad
a_tuple = 9,
a_long_triple = 7, 8, 9

# Good
a, b, c = 7, 8, 9

# Also good
def return_triple():
    return 7, 8, 9

a, b, c = return_triple()

# Good DSL examples
assert False, 'This test was suppose to fail'

print 'Put', 'spaces', 'between', 'these', 'words'
Tuple vs list
Prefer tuples over lists
When deciding which collection type to use prefer tuple, especially if that collection is not going to be changed.

# Bad
def compare_interests(other_persons_interests):
    interests = ['long walks on the beach', 'eating out', 'skateboarding']
    in_common = 0

    for interest in interests:
        if interest in other_persons_interests:
            in_common += 1

    return in_common

# Good
def compare_interests(other_persons_interests):
    interests = ('long walks on the beach', 'eating out', 'skateboarding')
    in_common = 0

    for interest in interests:
        if interest in other_persons_interests:
            in_common += 1

    return in_common

# Even if you are manipulating the contents of a collection still try to use a
# tuple.

# Good
def compare_interests(other_persons_interests):
    interests = ('long walks on the beach', 'eating out', 'skateboarding')
    in_common = ()

    for interest in interests:
        if interest in other_persons_interests:
            in_common += (interest,)

    return in_common
Imports
Imports should be at the top of a file
Imports should be grouped by standard library, third party libraries, and local application.
import os
import sys

import flask
from retrying import retry

from app.lib.klass import Klass
There are several ways to import a module in python

1) import package.a           # Absolute import
2) import package.a as a_mod  # Absolute import bound to an alias
3) from package import a      # Alternate absolute import
4) import a                   # Implicit relative import (deprecated, py2 only)
5) from . import a            # Explicit relative import
Avoid implicit relative import
You shouldn't be using implicit relative import (4th syntax), since it only works in python2 and runs the risk of clashing with other 3rd party modules.

Import what you need not the entire module
Doing so avoids unwanted side effects, especially when mocking in tests.

It also makes the current working module lighter, since it doesn't contain another whole module within itself.

If you want to avoid name clashing or the namespace's name adds to understanding intent, then use aliasing.

# not the best
import os

os.path(...)

# good
from os import path

path(...)

# super
from os import path as os_path
Refrain from using wild card, *, imports
It improves readability when we use explicit imports

Prefer single import statement with multiple objects when they come from the same module
# not the best
from os import path
from os import environ

# good
from os import path, environ
Do not import multiple unrelated modules in single import statement
# bad
import os, sys

# good
import os
import sys
Classes
When defining a class who does not inherit pass object as it's inherit class.
Avoid class attributes. Attributes should be limited to the scope of instances.
Use conventions of encapsulation when defining a class (on methods and attributes).
class Klass(object):
    # Public
    def public_method(self):
        self.public_attribute

    # Protected
    def _protected_method(self):
        self._protected_attribute

    # Private
    def __private_method(self):
        self.__private_attribute
It is bad practice to add attributes to an instance from outside of that class.
# bad
class Person(object): pass

pete = Person()
pete.name = 'Peter'

# acceptable
class Person(object):
    def give_name(self, name):
        self.name = name

pete = Person()
pete.give_name('Peter')
Private attributes are preferably defined with only one underscore instead of two.
Double underscores trigger name mangling. This makes debugging more difficult.

class Person(object):
    def __init__(self, name):
        self._private_name = name
Class Methods
As mentioned above, use conventions of encapsulation when defining class methods.

Prefer to define private static methods outside of the class in the surrounding scope.
Functions whom are just implementaion details should not be defined in the class.

Just put the function in the surrounding scope.

# bad
class Person(object):
    def eat(self, food):
        ...
        self.calories = self._add(self.calories, food.calories)
        ...

    @staticmethod
    def _add(a, b):
        return a + b

pete = Person()
pete._add(11, 12) # => 23

# good
class Person(object):
    def eat(self, food):
        ...
        self.calories = _add(self.calories, food.calories)
        ...

def _add(a,b):
    return a + b

pete = Person()
pete._add(11, 12) # => AttributeError
Exceptions
Catching Exceptions
Catch specific exceptions whenever possible
Do not use a bare except: clause.
# bad
try:
  # an exception occurs here
except:
  # exception handling

# good
try:
  # an exception occurs here
except ValueError:
  # exception handling

# acceptable
try:
  # an exception occurs here
except Exception:
  # exception handling
EAFP
EAFP = Easier to ask forgiveness than permission.

As seen in the conditions section, exception catching is preferred over checking an object before using.

Python's performance does not suffer when exception handling.

Do not to abuse this rule in your flow control.
Even though exception handling has been optimized in python do not use exceptions for you flow control.

# bad
def action():
    for i in items:
        if i == UNWANTED_ITEM:
            raise EXCEPTION("contains unwanted item")
        else:
            ...

# good
def action():
    for i in items:
        if i == wanted:
            return "contains unwanted item"
        else:
            ...
Catch expected exceptions locally
# bad
def has_wanted_key(a_dict, wanted_key):
    a_dict[wanted_key]
    return True

try:
    has_wanted_key(a_dict, a_key)
except KeyError:
    ...

# good
def has_wanted_key(a_dict, wanted_key):
    try:
        a_dict[wanted_key]
        return True
    except KeyError:
        return False

has_wanted_key(a_dict, a_key)
Custom Exceptions
Exception names should end with the word Error (MyCustomError)
Within a project define a base exception for all other custom exceptions to inherit from.
All exceptions should be grouped together in an exceptions module app.lib.exceptions.*
Regular Expressions
Use single quote raw strings (r'.+') for regex values.
Raw strings preserve escape characters.

This will save you from unexpected behavior.

Avoid star wild cards *, use plus + instead.
Bugs are caused because * also allows for zero occurrences.

Complex regex should be compiled before using.
The resulting variable's name should clearly define it's purpose.

# Bad
pattern = re.match(r'(?:3[01])|(?:[0-2]\d)-(?:1[0-2])|(?:0\d)-\d{4}$', '31-11-1985')
pattern.match('31-11-1985')

# Good
valid_date_pattern = re.compile(r'(?:3[01])|(?:[0-2]\d)-(?:1[0-2])|(?:0\d)-\d{4}$')
valid_date_pattern.match('31-11-1985')
Usage
Regex is not preferable in python.
There are plenty of tools which are preferred (i.e. in, startswith, endswith, isdigit, istitle, and more).

# Bad
is_bob_pattern = re.compile(r'^Bob')
is_bob_pattern.match(name)

has_a_qu_pattern = re.compile(r'qu', re.I)
has_a_qu_pattern.match(word)

# Good
name.startswith('Bob')

'qu' in word.lowercase()
Main Contributers:
Avraf
Eyalstoler

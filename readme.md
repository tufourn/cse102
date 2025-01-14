
# CSE102
Stuff that might be helpful for those taking CSE102 at MSU.

## General advice and best practices
* As of Spring 2024, the class assignments are done on Codio, and its code editor kind of sucks. I highly recommend that you download and use a proper Python IDE like PyCharm.
* Start working on projects early, and read through the entire project specifications to get an idea of how to program it before actually writing any code. Writing pseudocode can be helpful.
* If you forget how to do something, just do a quick Google search. It's much faster than trying to dig through ZyBooks. Or use ChatGPT, just don't get caught.
* Add comments and use blank lines to separate your code into sections. This makes your code easier to read and understand.
* Use descriptive names for your variables. I personally also like to use the `_list` postfix for list names to make it clear. For example, if you have a list of `customer_name`, name it `customer_name_list`.
* Run and test your code often to make sure it's behaving as expected. I've seen some students write hundreds of lines of code without running it once, and then wonder why the program doesn't work.

## How to use this document
This is meant to be a summary of key concepts and quick reference guide when you forget how to do something. It is **not a substitute** for reading the textbooks and watching the lecture.

## String formatting
Most of this class is about formatting and printing stuff out correctly, so it's important that you know how to do it with `f-strings`.

f-strings allows you to embed expressions inside string literals using curly braces `{}`. Here are some examples:
```python
# You'll often use f-strings inside print statements, but you can assign it to a variable as well
name = "Alice"
fstr = f"Hello {name}"    # Notice the f in front of the quotation mark
print(fstr)               # This will print out "Hello Alice"
```
```python
x = 1
y = 2
print(f"x={x} plus y={y} is z={x+y}") # This will print out "x=1 plus y=2 is z=3"
```

You can perform string formatting using a colon `:` inside the curly braces `{}` by following this format:

```
#     Before the colon is the expression, or the value you want to replace the curly braces with
#     |
{<expression>:<formatting_specifier>}
#                     |
#                     After the colon is the format specifier
```

[This](https://www.pythonmorsels.com/string-formatting/) is a cheatsheet containing most format specifiers in Python, but the most important ones that you're likely to encounter in class are shown below.

To specify the number of decimal places, use `.xf`, where `x` is the number of decimal places you want.
```python
pi = 3.141592653589
print(f"The value of pi is {pi:.2f}")    # This will print out "The value of pi is 3.14"
```

To specify the width and alignment, use `<direction><width>`, where `<direction>` is one of `<` for left aligned, `^` for center aligned, `>` for right aligned. If `<direction>` is omitted, it defaults to left aligned.
```python
name = "Alice"         #                 |123456789|
print(f"{name:9}")     # This prints out "Alice"
print(f"{name:>9}")    # This prints out "    Alice"
print(f"{name:^9}")    # This prints out "  Alice  "
```

You can even do both at the same time. In this example, `>9` means right aligned in 9 characters, `.2f` means displaying 2 decimal places
```python
pi = 3.141592653589   #                 |123456789|
print(f"{pi:>9.2f}")  # This prints out "     3.14"
```

## For loops
Let's say that you have a list of numbers, and you are asked to modify its elements, for example increment each number by 1.
```python
example_list = [0, 1, 2, 3]
```
If you do something like this, the list isn't modified at all, and you get the wrong result. The reason is a bit [complicated](https://stackoverflow.com/questions/14814771/in-a-python-for-loop-is-the-iteration-variable-a-reference-can-it-be-used-to).

```python
example_list = [0, 1, 2, 3]
for num in example_list:
    num += 1
print(example_list)
```
```python
# Expected output
[1, 2, 3, 4]
# Actual output
[0, 1, 2, 3]
```
The good news is, you don't really have to understand all that. All you need to know for this class is, when you want **to iterate and modify elements in a list, use `for i in range(n)`, then use square brackets `[]` to access and modify the elements**. If you only need to read values from a list without modifying it, then using `for x in lst` is fine.
```python
example_list = [0, 1, 2, 3]
for i in range(len(example_list)):
    example_list[i] += 1
print(example_list)
```
```python
# Expected output
[1, 2, 3, 4]
# Actual output
[1, 2, 3, 4]
```

## Parallel lists
### General Info
This is by far the most common pattern in CSE102 projects.
In this pattern, **multiple arrays of the same size** are aligned such that **the i-th element of each list represents an attribute of an object**.

For example, we can store the data of points on a grid as follows:
```python
point_id_list   = [1,     2,     3,     4   ] # ids must be unique
point_name_list = ['A',   'B',   'C',   'D' ] # names must be unique 
point_x_list    = [-1.0,  1.0,   1.0,   -1.0]
point_y_list    = [-1.0, -1.0,   1.0,   1.0 ]
```
In this example, all lists have a size of 4. The first element of each list represents point A, the second element of each list represents point B, and so on. Note that for this example, the elements in `point_id_list` and `point_name_list` must be unique, so we can reference them.

### Common operations
#### Modifying entries
To modify an entry in our database, use `.index()` to **find the index corresponding to that entry**, then use square brackets `[]` to **access and modify the corresponding element in the desired lists**.

For example, to modify the `X` and `Y` position of an existing point, given its `name`:
```python
point_to_modify = input("Enter name of point to modify: ")
point_new_x = float(input("Enter new X position for point: "))
point_new_y = float(input("Enter new Y position for point: "))

point_index = point_name_list.index(point_to_modify)  # Find index of point to modify
point_x_list[point_index] = point_new_x               # Change the point's X position
point_y_list[point_index] = point_new_y               # Change the point's Y position
```

#### Adding new entries
To add a new entry to our database, gather all the required information and use `.append()` to add them to the appropriate list. **Every list in the database must be updated so that they all have equal size**.

For example, to add a new point to our database:
```python
# Gather inputs from the user
new_point_id   = int(input("Enter new point id: "))
new_point_name = input("Enter new point name: "))
new_point_x    = float(input("Enter new point X position: "))
new_point_y    = float(input("Enter new point Y position: "))

# Append to database
point_id_list.append(new_point_id)
point_name_list.append(new_point_name)
point_x_list.append(new_point_x)
point_y_list.append(new_point_y)
```

However, the user might enter an `id` or `name` that already exists in the lists, which would break our system. To fix this, we'll typically use a function that generates an unique `id`,  and implement error checking to make sure the user enters an unique `name`.

#### Custom functions to generate attributes

In general, user shouldn't have to enter an `id` for a new point. Rather, an `id` should be generated and assigned. We'll use a custom function for this. Depending on your project specifications, this function might already be in the library that you're given, or you might have to implement them yourself.

For this example, we'll write a simple function that takes in a list of existing `id`, and returns the next integer as a new, unique `id`.

```python
# Note that the name of the paramater doesn't have to match the name of point_id_list
def get_next_id(id_list):
    # Reads the last id in the list, and use the next integer as our new id
    new_id = id_list[-1] + 1
    
    # If our new id already exists, keep incrementing until we get an unique value
    while new_id in id_list:
        new_id = new_id + 1

    return new_id
```

The code to add a new point becomes:
```python
# Gather inputs from the user
new_point_id   = get_next_id(point_id_list)      # Pass point_id_list to our custom function

new_point_name = input("Enter new point name: "))
new_point_x    = float(input("Enter new point X position: "))
new_point_y    = float(input("Enter new point Y position: "))

# Append to database
point_id_list.append(new_point_id)
point_name_list.append(new_point_name)
point_x_list.append(new_point_x)
point_y_list.append(new_point_y)
```

#### Error checking

When the user enters an invalid input (i.e. value already exists, value entered is too big), you'll often be asked to prompt the user for input until they enter a valid one.

To keep prompting the user for input is simple, just use a `while` loop. Here's the pseudocode for how it's usually done:

```
user_value = input("Enter value")     # Get input from user
while (user_value IS INVALID):        # This while loop will repeat itself until user_value is valid
    user_value = input("Enter value") # Reprompt the user for input
```

For our example, to make sure the `name` entered by the user is unique, our code will look like this:
```python
# Gather inputs from the user
new_point_id   = get_next_id(point_id_list)

new_point_name = input("Enter new point name: "))
while (new_point_name in point_name_list):               # When name already exists, reprompt
    new_point_name = input("Enter new point name: "))    # Reprompt the until a valid name is entered
    
new_point_x    = float(input("Enter new point X position: "))
new_point_y    = float(input("Enter new point Y position: "))

# Append to database
point_id_list.append(new_point_id)
point_name_list.append(new_point_name)
point_x_list.append(new_point_x)
point_y_list.append(new_point_y)
```

#### Filtering data

Sometimes you'll be asked to get a list of entries that meet a specific criteria. To do this, first **initialize a new empty list**, then **traverse through the lists in our database**. Whenever an entry that meets the criteria is encountered, **append it to our newly initialized list**.

For example, if we want to get a list containing `name` of points that have positive X and positive Y:
```python
filtered_point_name_list = []    # Initialize a new, empty list

# Get the length of a list in our database (they all have the same length), then iterate over that range
for i in range(len(point_id_list)):
    # Recall that the i-th element of each list represents an attribute of an object
    # point_x_list[i], point_y_list[i], point_name_list[i] all describe a single point
    
    # Check if the position of point at this index meets our criteria
    if (point_x_list[i] > 0 and point_y_list[i] > 0):
        # If the criteria is met, append the name to our filtered list
        filtered_point_name_list.append(point_name_list[i])
```

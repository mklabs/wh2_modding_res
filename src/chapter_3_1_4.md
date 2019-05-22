# Variables & Types

*NOTE: I recommend you have the Devtool Console handy to mess around with some of the stuff covered here.*

Following the concept of chunks and statements, the next most important subject to cover is variables, and types.

**Variable**: A custom-made keyword that can be used to represent a value
**Type**: The sort of value that is in a variable. Lua types include: boolean, string, number, table, nil, function. There are more, but we won't need them.

But before going deeper into a definition, let's look at an example:

```lua
a = 10
b = 5
c = a + b
```

We can see that each of those letters in the chunk above is a **variable**, which are each assigned a **value**. In this case, variable **a** is assigned the value **10**, variable **b** is assigned the value **5**, and the variable **c** is assigned the **value of a plus the value of b**. This instance (c = a + b) works a lot like algebra - you aren't making "c = ab", but you're adding the value of a (10) to the value of b (5), to get the result of **15**.

The coolest thing about variables is their dynamism. They can be assigned to any type, change to any type, and they can infinitely change values. Let's take the above code, and mess with it a little bit.

*NOTE: From this point on, you'll see → after out() calls. This is to specify what text is actually output.*

```lua
a = 10
b = 5
c = a + b

out(a) → 10
out(b) → 5
out(c) → 15

a = 25
out(a) → 25

out(c) → 15

c = a + b
out(c) → 30
```

I wanted to point out an important thing with this example. If a variable is assigned to the values of some other variables (c = a + b), it will do the calculation and assignment the **moment** that the statement is called. If either variable (a or b) is changed after the assignment statement, the assigned variable's (c) value (15) won't change. It takes the *variable's value*, not the *variable itself*.

So, we see here that variables can be numbers, which is one of the primary types in Lua.

**Number**: *any* numerical value, within a specific number range (a much bigger number than needs to be worried about)

As we saw above, we can perform some basic arithmetic on numbers. 

```lua
a = 5
b = 2 - 0.34
c = a + b
d = c * 2
e = d / 3

out(c) → 7.56
out(d) → 15.12
out(e) → 5.04
```

**String**: *any* alphanumerical value, within quotation marks ("" or ''). Similar to numbers, size *does not matter*, Lua can handle millions of characters within a string.

Like arithmetic for numbers, one can concatenate strings (along with many other stuffs!) to combine several, using the `..` operator.

```lua
	a = “This “; b = “is “; c = “an “; d = “example.”
	out(a .. b .. c .. d) → This is an example.
```

*NOTE: Yes, you can have several statements on the same line in Lua! The semi-colons are not necessary, I added them for extra readability.*

While on the subject, you can concat (cool-kid for concatenate) string literals and variables.

```lua
    a = "example"
    out("This is an " .. a) → This is an example
```

**Boolean**: a `true` or `false` value. 

```lua
    a = false
    out(a) → false
    out(a == false) → true
```

The above probably looks a little weird, but that's because we're learning a new operator! The `==` operator checks for *equality*, whereas the `=` operator *assigns a value*. The final statement above reads "output the result of a == false". Since `a` DOES equal false, it prints "true".

It's very important to note that any variable can be *treated* as a boolean, such as when using the "if something then" statement, or by comparing it to a boolean. If a non-boolean variable is treated as a boolean, it will be converted to `false` if the value is `nil` or `false`; otherwise, it will be converted to `true`.

```lua
    a = 0
    b = nil
    out(a) → 0
    out(a == false) → false 
    out(b == false) → true
```

In this case, `a` does NOT equal `false`, because it has a value to it that isn't `false` or `nil`. By the same card, `b` DOES equal `false`.

You're probably asking by now, what IS nil? I've mentioned it a number of times already, you're probably ready to lear about it. Here we go!

**Nil**: the absence of data.

If one were to try to print a *non-existent* variable, the result would be `nil`. That's because there **is** no data for that variable.

```lua
    out(a) → nil
    a = 5
    out(a) → 5
```

Nil is a good way to check whether or not something exists, and we'll, towards the bottom of this lesson, talk about how to type-check this data in our scripts to see if variables are NOT nil, and thus exist.

Nil is also a good way to clear away variables that you don't want to use anymore. There's a more realistic way of doing it, and this probably won't be necessary for most of what you will be doing, but I like to give a full knowledge.

```lua
    a = 5
    out(a) → 5
    a = nil -- Memory deallocated, bye "a"!
    out(a) → nil
```

Lastly, it's important to note that `nil` is not the same as `null`, though they look quick similar. We'll cover that soon-ish, when we get into more CA script interfaces (like FACTION_SCRIPT_INTERFACE that we saw a few lessons ago).

**Function**: a definition for a chunk that can be called at any point, after being defined.

There are two types of functions - an anonymous function, and a named function. An anonymous function has no name (clearly), so it cannot be called further on in the script. We use anonymous functions relatively frequently, but our focus now will be on named functions, which are functions assigned to a variable.

```lua
    a = 10
    example = out
    example(a) → 10
```

Yes, that's valid! You can actually assign a variable to a function like, it's pretty cool! We have syntactic sugar for assigning a variable to a function, however, and it's more common to see script like that:

```lua
    function example(text)
        out(text)
    end

    example("Testing!") → Testing!

    example("A Cooler Test!") → A Cooler Test!
```

As an added bonus, we're getting a quick sneak peak at one of the upcoming tutorials: parameters and arguments, for functions. In the above function, "text" is a parameter for `example()`, which is then passed along to `out` and triggers that function within the chunk.

**Table**: a collection of more than one piece of data.

There are several types of tables, which we'll cover later on. I'd like to first look at the *array* version of tables, the simplest and most common.

```lua
    a = 5
    b = 10
    c = 17

    table = {
        a,
        b,
        c
    } -- assign the three *values* of the variables to the table
    
    out(table[1]) → 5 -- Lua tables start at index 1!
    out(table[2]) → 10
    out(table[3]) → 17
    out(table[4]) → nil -- doesn't exist!
```

Arrays are numbered automatically, so it really reads "the first index of table is 5, the second index of table is 10", and so on. You *access* those indexes by using the `\[num\]` operator, which we'll look further at in the next tables tutorial later on. The reason this works, however, is because Lua is automatically numbering these values, giving them a *key*. When you make an array like that, Lua reads it more like the following:

```lua
    a = 5
    b = 10
    c = 17

    table = {
        [1] = a,
        [2] = b,
        [3] = c
    }
```

The other major type of tables is a **map**, which works by manually assigning the key of a value instead of letting Lua automatically number them, like in arrays.

```lua
    a = 5
    b = 10
    c = 15

    table = {
        ["five"] = a,
        ["ten"] = b,
        ["fifteen"] = c
    }

    out(table["five"]) → 5
    out(table["ten"]) → 10
    out(table["fifteen"]) → 15
```

Maps will come into some great use pretty soon, but for now I just wanted to introduce you to how they look.

For now, you've done well, and I haven't even challenged you yet!

#### Challenge 1

Mess with everything in this tutorial using the Devtool Console. Try to grasp the different native types of Lua, and make sure you understand the difference between the *assignment* operator and the *equality* operator. There's not really a good challenge to do here, but I do seriously suggest you just mess around with the stuff here. Try some different things like concatenating strings, adding numbers, messing with basic functions, stuff like that. Have fun!

#### Type-Checking

And before we say goodbye, let's look at a few more CA functions. These ones can type-check for us, seeing if a variable is, for instance, a string.

The list is as follows:
- is_nil(variable)
- is_string(variable)
- is_number(variable)
- is_boolean(variable)
- is_function(variable)
- is_table(variable)

```lua
    a = 5
    b = nil
    c = {1, 2, 3, 5, 70}
    function d() end

    out(is_number(a)) → true    
    out(is_nil(b)) → true
    out(is_table(c)) → true
    out(is_function(d)) → true
```
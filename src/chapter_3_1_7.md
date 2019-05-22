# Functions, Arguments & Parameters (Also, Returns)

Welcome back! On today's lesson, we're going to cover one of the most fundamental types in Lua much further, and that type is, if you can read, *functions*. Everybody loves functions.

Functions are a special data type in Lua. Functions have two specific stages: **definition**, and **call**. A function can be defined as many times as one would like, but it will not do anything until the function is *called*. Quick look, followed by a breakdown:

```lua
    -- DEFINE the function, which we can call and trigger later on, as much as we'd like
    function example_function()
        out("This is your example function") -- at this point, nothing is output
    end

    -- actually calling the function to trigger the out!
    example_function() → This is your example function
```

And that's the basics of it! We should note that in this example, `example_function` is actually a variable name. The above is syntactic sugar for the following:

```lua
    example_function = function()
        out("This is your example function")
    end
```

But the other way is a lot more common, and probably prettier for all eyes. On the case of syntactic sugar, you can simply put the keyword `local` prior to the keyword `function`, and it will make it a local variable. I'll be doing that from now on, to beat that idea in your own head.

Since the name of a function is just a variable, it can be redefined, and even as a different type. If a variable is defined more than once, the latest definition will "win".

```lua
    local function example_function()
        out("This is your example function")
    end

    example_function = 10 -- Why would ANYONE do this?!

    example_function() → attempt to call global `example_function`, a number value
```

The term "call" here is what I mentioned earlier - the *call* phase of a function. A call to a function is triggered by those two brackets - `()`.

At this point, we have a function that prints the same text over and over again. Let's give it some way to change the text based on some dynamism!

```lua
    local function example_function(text)
        out(text)
    end

    example_function("Testing") → Testing
    example_function("Example!") → Example!
```

The term "text" is a `local variable` for that function, available in the entirety of that function's scope. It goes "out of scope" (as the cool kids say) by the `end` keyword. This is another clever shorthand that the Lua languages gives to us. Use it!

A clarity-for-clarity's-sake note here: in the above example, "text" is a *parameter*, which is the fancy word for a local variable assigned to the function's scope. The string "Testing", or the string "Example!", are *arguments*, which is a variable or value that is "passed" into the function and takes the place of the parameter at runtime. We can think of a parameter as a "fill-in-the-blank" space on an entry form, and the argument as whatever we fill in there.

One final thing we should know about before we go on is *return values*. In the above, we're just passing stuff *into* a function. But what if I want to take stuff *out* of a function? Well, return values, of course!

The keyword `return` is neat, and two-fold. Its first, and primary purpose, is to take whatever is to the right of it and shoot it back out, if the function is called. Its second, and still very-important, purpose is to stop the function once the keyword is called. Let's take a look at a common example of return values:

```lua
    local function double(num)
        -- if the parameter is not a number type, then output some text and then STOP the function, to prevent it from going on
        if not is_number(num) then
            out("double() called, but you didn't supply a number! Aborting function!")
            return
        end

        -- if the parameter is a number type, then double it and return it! (we can safely assume that "num" IS a number, because otherwise the function wouldn't go this far)
        local val = num * 2
        return val
    end

    local a = 2
    out(a) → 2

    -- define a local variable as the RESULT of "double(a)"
    local b = double(a)
    out(b) → 4
```

You can return any type, and you can even return multiple values at once. Say you wanted to take the above function, but instead of just doubling the number, you *also* tripled it, and wanted to get both values.

```lua
    local function double_and_triple(num)
        -- if the parameter is not a number type, then output some text and then STOP the function, to prevent it from going on
        if not is_number(num) then
            out("double_and_triple() called, but you didn't supply a number! Aborting function!")
            return
        end

        local double = num * 2
        local triple = num * 3

        -- this is it, just separate the return values or variables with a comma!
        return double, triple
    end

    local number = 5

    local d, t = double_and_triple(number)
    out(d) → 10
    out(t) → 15
```

I'd like to point out that I'm changing the names of my variables to point out that *the name of parameters can be whatever you want*. Don't be confused by me defining the function as `local function double_and_triple(num)` and calling it using `double_and_triple(number)`. `num` is a local variable, available only in the SCOPE of the function, whereas `number` is a *different* local variable, available to a higher scope but below the function definition.

At this point, we've got all the background knowledge really needed to start doing some more cool stuff! In the next lesson, we'll learn more about script interfaces, we'll go further in-depth about listeners (which we briefly used in lesson #2), and we'll talk about commands and how to check for existing ones! And you'll have A CHALLENGE!

WOO!
# Functions, Arguments and Parameters

Welcome back! On today's lesson, we're going to cover one of the most fundamental types in Lua much further, and that type is, if you can read, *functions*. Everybody loves functions.

Functions are a special data type in Lua. Functions have two specific stages: **definition**, and **call**. A function can be defined as many times as one would like, but it will not do anything until the function is *called*. Quick look, followed by a breakdown:

```
    -- DEFINE the function, which we can call and trigger later on, as much as we'd like
    function example_function()
        out("This is your example function") -- at this point, nothing is output
    end

    -- actually calling the function to trigger the out!
    example_function() → This is your example function
```

And that's the basics of it! We should note that in this example, `example_function` is actually a variable name. The above is syntactic sugar for the following:

```
    example_function = function()
        out("This is your example function")
    end
```

But the other way is a lot more common, and probably prettier for all eyes. On the case of syntactic sugar, you can simply put the keyword `local` prior to the keyword `function`, and it will make it a local variable. I'll be doing that from now on, to beat that idea in your own head.

Since the name of a function is just a variable, it can be redefined, and even as a different type. If a variable is defined more than once, the latest definition will "win".

```
    local function example_function()
        out("This is your example function")
    end

    example_function = 10 -- Why would ANYONE do this?!

    example_function() → attempt to call global `example_function`, a number value
```

The term "call" here is what I mentioned earlier - the *call* phase of a function. A call to a function is triggered by those two brackets - `()`.

At this point, we have a function that prints the same text over and over again. Let's give it some way to change the text based on some dynamism!

```
    local function example_function(text)
        out(text)
    end

    example_function("Testing") → Testing
    example_function("Example!") → Example!
```

The term "text" is a `local variable` for that function, available in the entirety of that function's scope. It goes "out of scope" (as the cool kids say) by the `end` keyword. This is another clever shorthand that the Lua languages gives to us. Use it!

A clarity-for-clarity's-sake note here: in the above example, "text" is a *parameter*, which is the fancy word for a local variable assigned to the function's scope. The string "Testing", or the string "Example!", are *arguments*, which is a variable or value that is "passed" into the function and takes the place of the parameter at runtime. We can think of a parameter as a "fill-in-the-blank" space on an entry form, and the argument as whatever we fill in there.

At this point, we've got all the background knowledge really needed to start doing some more cool stuff! In the next lesson, we'll learn more about script interfaces, we'll go further in-depth about listeners (which we briefly used in lesson #2), and we'll talk about commands and how to check for existing ones! And you'll have A CHALLENGE!

WOO!
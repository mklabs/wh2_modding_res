# Scopes & Stuff

A super important concept in Lua is the concept of *scope*. Every single variable made has a *scope*, which is used to define **where** a variable can be accessed.

By default, all variables are `global`, which means they're in the entire **global environment**. The global environment is a slightly-fancy way of saying "available within the entire program". When writing Total War scripts, it's a little suboptimal to use global variables, for a few reasons -
    1) What if someone else wants to use a variable named `a`, and you do as well? This would force you to have a fully unique name for every single variable everywhere. Kinda a pain!
    2) Saving that variable globally is taking up memory, since that variable will never be destroyed. It will take a lot of work to actually cause performance issues, but it's good to be mindful of.
    3) What if we want our variable to be a secret? Huh? They can't get our birthday present! *gollum*

And all this is where the keyword `local` comes in. The `local` keyword defines a specific scope for a variable, and prevents them from being a global variable.

```
function example()
    local var = "Hello World!"
    out(var)
end

example() → Hello World!
out(var) → nil -- the variable "var" doesn't exist here!
```

In this example, the variable `var` is defined within the function `example`'s definition. That means that `var` is only available from *where it is defined* (ie. local var =) to *where the scope ends*. In this case, the scope *ends* at the `end` keyword.

This is as good a time as any to look at the other ways scope can be defined. A variable can be scoped to the *root* of a script, or any number of nestled scopes within that root.

Other key-words other than `function` that define a scope (and their end point) are as follows:
- do --[[ the scope ]] end
- function function_name() [[ the scope ]] end
- if whatever then [[ the scope ]] end
- while whatever then [[ the scope ]] end

Let's take a slightly-complicated look at that:
```
local a = 5 -- visible to everything BELOW this line!

function example_thing()
    local b = a -- visible to everything BELOW this line, until the "end" that matches "function"
    do 
        local c = 15 -- visible to everything BELOW this line, until the "end" that matches "do"
        out(c)
        out(b) 
    end
    out(b)
    -- c is NOT available here!
end

-- b and c are NOT available here!
out(a) → 5
```

This is a good point as any to explain two things, and issue two directives:
1) ALWAYS use the keyword `local`.
2) ALWAYS use good indentations!

Be smart with where exactly the term "local" is used - you'll pick up how later on as you go and pick my brain, but for now, understand that you must ALWAYS put the keyword `local` before a variable!

As you can see with the example_thing() function above, proper indentation is a GREAT way to visibly see scopes and quickly tell where a variable is and isn't available.
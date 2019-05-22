# Expressions & Operators

As we've seen, Lua is made up of `chunks` and `statements`. Statements can contain variables, and all variables have a type, though it can change at any point.

**Expressions** are the section of a statement that, essentially, tell Lua *what to do*. They come in a billion different shapes and sizes. For now, we'll cover all the basics you'll need, and some basics you really don't need but I'll hand it to you anyway, and we'll go back and review some older stuff. Here we go!

#### Math Operators

Sweet. This is what all of those quizzes in grade school trained you for. We have the big four from algebra - addition, subtraction, multiplication, and division.

```lua
    out(2 + 3) → 5
    out(5 - 1) → 4
    out(16 * 3) → 48
    out(9 / 3) → 3
```

We can also use a `-` to *negate* a value, or, in simple terms, turn a positive number into a negative and vice versa.

```lua
    a = 10
    b = -20
    out( -a ) → -10
    out( -b ) → 20
```

We also have a couple other fun math things. We can raise numbers to any power, using an exponential expression. 

```lua
    a = 2
    b = 4
    c = 3
    out( a^a ) → 4
    out( b^c ) → 64
    out( 64^0.5 ) → 8 -- getting the square root of a number!
```

We can also use some modulo math. It's a way of dividing two numbers that gives you the amount *remaining*, and it's a great way to read if a number is a *multiple* of another.

```lua
    out(8 % 2) → 0 -- 8/2 = 4, and divides evenly, so nothing is remaining!
    out(7 % 2) → 1 -- 7/2. "6/2" divides evenly, so we subtract 6 from 7, and get "1" as the remainder!
    out(8 % 3) → 2 -- 8/3. "6/3" divides evenly, so we subtract 6 from 8, and get "2" as the remainder.
```

As said, it's a great way to read if a number is a multiple of another. If a modulo expression returns "0", that means it divided evenly, and is therefore a multiple of the number divided by.

And that's math!

#### Comparison Operators

This sounds a lot more complicated than "math", eh? It's not that complicated - we're comparing how two variables relate to one another, using expressions we've all seen before. The list of comparison operators are as follows:

- < -- "less than"
- \> -- "greater than"
- <= -- "less than or equal to"
- \>= "greater than or equal to"
- == "equal to"
- ~= "not equal to"

All of these expressions result in a boolean value.

```lua
    a = 10
    b = 7 + 3
    out(a == b) → true -- they ARE equal!
    out(a ~= b) → false -- they ARE NOT not equal (weird, I know)!

    c = 8
    d = 7
    
    out(c > d) → true -- 8 is greater than 7
    out(c < d) → false -- 8 is NOT less than 7
```

It's important to note that Lua will consider different types to be different values. A string and a number will always be different. Lua will throw an error if you attempt to compare - using less-than or greater-than - any two separate types. Lua will not throw an error for "==" and "~=", however, and if they are separate types, they will always be inequal.

```lua
    a = 15
    b = "15"
    out(a == b) → false -- they are different types!
    out(a >= b) → error (attempt to compare number with string)
```

Do note that you can compare strings using greater or less than, but it compares the alphanumerical order of the first character. “0” is less than every number or letter, whereas “a” is greater than every number and less than every letter, and “z” is greater than every number or letter. Again, not really important, but you came here to read me ramble on about random stuff.

And that ends our comparison operators!

#### Logical Operators

Next up are our logical operators! We have three operators to concern ourselves with here - **and**, **or**, and **not**. 

*Not*, as a logical operator, is pretty simple and super handy. It basically converts the value to the right of it into a boolean, and reverses it. Any value other than `nil` or `false` will be converted into `false`, and `nil` or `false` will be converted to `true`. Let's take a look:

```lua
    a = true
    b = nil
    out(not b) → true -- prints a double negative - not false, so true
    out(not a) → false -- reverses the value of "true"

    c = 100
    out(c) → 100
    out(not not c) → true -- c is a number, "not c" would become "false", and "not false" would become "true"
```

The operators **and** and **or** are the hearts of a lot of logic in Lua. "If this and this, then do this". 

```lua
    a = 15
    b = false
    if a == 15 and not b then -- "if a is equal to the number fifteen, and "not b" returns true, then ... "
        out("It does the thing!") -- " ... print the text! "
    end
```

You'll practically always see these two used in this way, for *conditional statements* like that - if these conditions work out, then do this other thing.

#### Homeless Operators

I don't really have a category for these two operators, but they're incredibly important.

First off, we have **concatenation**, which we've seen before. It's a way to add two strings together, and make them into one happy family, using a simple pair of dots.

```lua
    a = "Hello"
    b = "World!"
    out(a .. " " .. b) → Hello World!
```

A couple of notes real quick - I have the blank space in a string in the middle, to make sure it doesn't print "HelloWorld!". Also, I like to add spaces on either side of my concatenation operator, but that's personal taste, this would also be valid:

```lua
    out(a.." "..b) → Hello World!
```

And lastly, we have one more operator, and this time for the table type. The character `#` can be used to read the number of entries within an array. It's pretty awesome, let me show you:

```lua
    table = {"this", "is", "an", "example", "array"}
    out(#table) → 5 -- there are five strings within the table!
```

Please note that you really shouldn't use the `#` operator if the table is not definitely an array. I'll show you why using it on maps might mess something up:

```lua
    table = {
        [1] = "test",
        [2] = "example",
        [5] = "boom",
        [7000] = "woo",
        [3] = "argh"
    }

    out(#table) → 7000
```

In technicalities, it's doing that because it's reading the *highest index in that table*, or the highest numerical key. Those are assigned automatically, in order, for arrays; in maps, there's no such guarantee.

#### Challenge 1

As with last, no challenge! We have only a couple more lessons to do before we get into the REAL nitty-gritty, and once we do there will be a lot of nice challenges. For now, keep messing with this stuff in the DevTool Console, test around.
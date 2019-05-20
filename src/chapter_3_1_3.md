# Chunks & Statements

The first two terms we'll look at are __chunks__ and __statements__. When Lua is brought to its basic skeletal structure, that's all it is - chunks of statements.

**Statement**: A declaration, a thing-to-be-done, a command that Lua will run
**Chunk**: A sequence of statements, from an entire file to a single line of data

Every chunk is a to-do list, and a statement is an objective on that list. You can have to-do lists of all shapes and sizes, and each thing that needs to be done can be small - "brushing your teet" - or huge - "spend way too much money on transmission repairs". No, I'm not still salty about the money I spent on transmission repairs, why do you ask?

The Lua interpreter divides everything it takes into specific chunks, which we'll look at later on. For now, we understand the term chunk - a to-do list.

Within each chunk is one or more statements. A statement is one coherent thought or action within Lua. Something like this can be one statement:
```
a = 1
```

Or, this can be one statement:
```
a = 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 - 1 / 70 * 300 + 5180 - 418 + 405
```

That is, one bit of data that Lua has to evaluate and run.

Remember the term "chunk" - we'll be covering how they're defined later on, when we look at some more keywords.
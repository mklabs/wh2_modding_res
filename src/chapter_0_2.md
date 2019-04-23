# Converting to Markdown

### Markdown Conversion

This page is intended as a very quick guide on how to convert your tutorial into the language used here, markdown. Markdown is super simple and made to be incredibly simple.


#### Standard Text

These four are the backbone of any good tutorial.

Italics - \*text\*

*this is an example*

Bold - \*\*text\*\*

**this is an example**

Italics & Bold - \*\*\*text\*\*\*

***this is an example***

Strikethroughs - \~\~text\~\~

~~this is an example!~~

#### Blockquotes

Blockquotes are a great way of showing specific code or db stuffs. All you have to do is put a ">" preceding a paragraph.

> This is a blockquote.
> 
> And this is the blockquote's end.

> You can use most markdown elements in a blockquote, but I won't cover the possibilities here.

> You can also make nestled blockquotes.
>
>> Like this.
>>>> And, this.

#### Lists

Super simple, like the rest of this language. To make a nice bulleted list, use "-" prior to the item, with a line break in between.

- Example list.
- Look at my shiny buttons.
    - You can even indent.
    You can also randomly not use a bullet to say something.
- And then use a bullet again.

If you want to number the list, just put "1." prior to the first item, and then iterate the number for each new list item.

1. First thing on the agenda - put 1.
2. Shortly after that, put 2.
    I think you get the example by now.
3. One more, for good measure.

#### Codeblocks

For any written code, you can use a single backtick/grave (\`) on both sides of the text in order to write code inline. Kinda like `this`.

If you want to make blocks of code, you can use three of the above, on both sides of the text.
```lua
local example = "text"

function do_stuff()
    print(example)
end

do_stuff()
```

#### Line Breaks

If you want to make big lines across the screen, use three (or more) hyphens, asterisks, or underscores, in a line by themselves.

***

#### Links and Images

Links are pretty simple, and can have a specified tooltips for when they're hovered over, or converted into text and made into a hyperlink, or can remain a standard clickable link.

To make a standard clickable link, wrap the URL with "<>". 

<https://www.google.com>

To give a link a title that replaces the URL, first put the title within "[]", and have it immediately followed by "()".

\[example title\]\(https://www.google.com\)

[example title](https://www.google.com)

Images are much the same as above, use a "!" preceding the title and the link designation.

\!\[example image\]\(https://cdn.discordapp.com/emojis/460756324729356299.png?v=1\)

![example image](https://cdn.discordapp.com/emojis/460756324729356299.png?v=1)

For either, if you want alternate text to popup when hovering over the element, put the alternate text within the "()", after the link, and in quotation marks, like so:

\[example title\]\(https://www.google.com "This Is Google!"\)

[example title](https://www.google.com "This Is Google!")

#### Headings

You can set six different bolded-sizes for headers, for visual distinction. A header is simply a number sign (#) followed by the text that is being head'ed. There can be one number sign, up to six number signs, preceding the text. One number sign is the largest, six the smallest.

# One Number Sign
## Two Number Signs
### Three Number Signs
#### Four Number Signs
##### Five Number Signs
###### Six Number Signs
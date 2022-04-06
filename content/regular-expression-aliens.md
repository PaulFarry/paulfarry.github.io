## History
When Regular Expressions were created for more detail please read this [Wikipedia](https://en.wikipedia.org/wiki/Regular_expression)  article. I think Stephen Cole Keene may have been visited by aliens because the syntax is just so non human readable. Sure it is super powerful and does many amazing things, but if you don't use it everyday, when you come to do something you'll be in for a world of hurt.

## Problem
So for work I've been trying to build a tokeniser for replacement of parameters in text from a series of steps that will build up a series of replacement values along the pipeline. When the step arrives, it passes some tokenised text to the parser, it finds the tokens and replaces them with the values. Fairly simple...but we have a problem, if there is more than 1 token on a line it doesn't find them all it only finds the first one. Enter the world of Greedy and Lazy


### Sample Block
This is a \{\{block\}\} to be \{\{replaced\}\}. If we have a \{\{third\}\} value strange things happen  
until we \{\{start\}\} a new line.


(Multi-line enabled)

## First Draft
`\{\{(.*)\}\}`


## Solution
`\{\{(.*?)\}\}`

Note the simple addition of the ? makes it all work


## Explanation

```

\{\{(.*?)\}\}

\{ matches the character { with ASCII 123 literally (case sensitive)
\{ matches the character { with ASCII 123 literally (case sensitive)
  1st Capturing Group (.*?)
    . matches any character (except for line terminators)
    *? matches the previous token between zero and unlimited times, as few times as possible, expanding as needed (lazy)
\} matches the character } with Ascii 125 literally (case sensitive)
\} matches the character } with ASCII 125 literally (case sensitive)

Global pattern flags 
g modifier: global. All matches (don't return after first match)
m modifier: multi line. Causes ^ and $ to match the begin/end of each line (not only begin/end of string)

```

This information from [RegEx101](https://regex101.com/r/0aLIsa/1)
---
layout: post
title:  "Meet OutcomeScript"
description: "Outlining default language syntax for user files"
tags:
  - devlog
permalink: "/blog/meet-outcomescript"
published: true
---

<meta property="og:image" content="https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80"/>

As internally the engine will be using a custom command processor, it only
makes sense to figure out some sort of way to represent the logic "on paper".

For lack of a better name I named the new format `OutcomeScript`, with the
file extension probably being something like `.os` or `.osc`.

Anyway, I'd like to talk about some of the things I've been thinking about
related to custom file syntax and logic processing in general.


## Something something reinventing the wheel

Why not use an existing solution? An existing scripting language? 
Why not just embed an existing library into the project?
Well, that would be the first thing I'd do, trust me. Problem is 
it's not that easy.

First of all there are not that many existing solutions out there.
As you may be aware, for this project I'm working almost exclusively with
the Rust programming language. As such, there are not as many libraries
available for Rust as for other older languages like C/C++ or python, 
especially when it comes to more obscure things like command-based 
interpreters.

I've only found 2 Rust projects that are similar to what I'm aiming for
with this project, and they are still quite new: duckscript and molt.
The latter is an attempt at implementing Tcl in Rust, more or less, 
and as such is very interesting. Also the code documentation is godlike
for that one!

The former one, duckscript, offers a slightly different take yet, focusing
much of the effort on extensibility of the language itself. I like 
some parts of the design of duckscript over Tcl, like using `end`
keyword to signal end of procedures and if/else blocks.




 
## Drawing inspiration from languages like Tcl

Tcl, as it's name suggests, is not a terribly sofisticated thing when you
compare it to other languages out there. But that's almost the point.
It's simple, and sort of elegant. 

`OutcomeScript` as a tool language will probably inherit some of the 
rules from Tcl. Most obvious thing like treating everything as 
commands has already been established. What I'm looking at right now
is some of the more concrete syntax stuff, like notation and variable
substitution. 


## I want custom features

Problem with using solutions I mentioned before is that they don't 
provide some of the functionality I'm after. 


### Single model, multiple runtimes




```

proc test_procedure
	print "printing from inside test_procedure"
	print "about to end the procedure"
end

```


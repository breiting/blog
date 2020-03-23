---
title:  "Pimp your code"
date:   2018-08-13
tags: ['coding', 'style','editor']
---

Joining a new software development team typically means new coding guidelines
and rules. If you are lucky and no guidelines are in place, it is your
*obligation* to take care and define them. To my opinion, coding guidelines make
sense and are important for the product quality. They deliver a good foundation
for collaborative developments and easy maintenance. I am not talking about a
50 pager, where everything is defined - no - only the most important things
should be part of the guideline, for example:

* Indentation
* Placement of braces
* Important naming conventions (depending on the programming language)
* Namespaces
* Source code documentation
* Header for each file (e.g. licensing)

Make sure that the team agrees on those items, and write them down, either on
the team’s wiki or directly as a text file in the code repository. This also
makes it easy for new hires to start quickly and get comfortable with the new
environment.

A tool which comes in pretty handy is **Artistic Style** ([astyle](http://astyle.sourceforge.net/)). Artistic Style
is a source code indenter, formatter, and beautifier for the C, C++, C++/CLI,
Objective‑C, C# and Java programming languages. It is available for
different platforms (e.g. Windows, Linux, MacOS) and can also be easily
integrated into your favorite development IDE.

Astyle accepts command line parameters which control the behaviour of the cod
formatting engine. An elaborate documentation can be found [ here ](http://astyle.sourceforge.net/astyle.html). For Linux and
MacOS-based systems, you can create a .astylerc file in your home directory
which is taken in case of no command line parameters (pretty handy!).

To make sure that your code is always checked in according to the defined rules, you can also define a pre-checkin hook
in your source control management tool. Your colleagues will thank you, and the repository always is inline with the
team's coding guidelines.

My experiences have shown, that a defined set of foundational rules helps the whole team by making the code more readable and  maintainable. Using a tool such as Artistic Style tool is pretty handy and helps to keep the code clean.

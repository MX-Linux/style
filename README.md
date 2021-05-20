# Style Guide for MX apps 

The main purpose of using a specific style is to standardize the code to increase readability and avoid confusions.

Main principle: we should always choose clarity over saving keystrokes.

## Qt/C++ apps
We are mostly following https://www.kernel.org/doc/html/v4.10/process/coding-style.html with the notable exception of using 4 blank spaces for indentation. Here are some relevant/important parts:

1) **Indentation** 

We use 4 blank spaces for each level of indentation.

Don’t put multiple statements on a single line unless you have something to hide:
```
if (condition) do_this;
    do_something_everytime;
```
Don’t put multiple assignments on a single line either. 

2) **Breaking long lines and strings**

Try to limit lines to 100 characters at most. Lines that need to exceed 100 characters must not exceed 120 characters.
We make exceptions for legacy translated texts and block of text that makes sense to be presented on one line (e.g., a bash command that we cannot split on two lines without reducing readability).

2.1) **Breaking conditional statements into multiple lines**
For veryshort statements in conditionals, it is acceptable to have a compact format where the statement is on the same line as the conditional:
```
if (conditional) do_this();

if (condition1) do_this();
else if (condition2) do_this();
else if (condition3) do_this();
else do_something_else();
```
Lines of this nature are not to exceed 100 characters.
If any conditional check has braces, the following conditionals within the if...else if...else structure must also have braces, regardless of line length:
```
if (condition1) do_this();
else if (condition2) do_this();
else if (condition3) {
    do_something_else_that_has_a_very_long_name();
} else {
    do_something_else();
}
```

3) **Placing braces and spaces**

Put the opening brace last on the line, and put the closing brace first:
```
if (condition) {
    action();
}
```

3.1) **Spaces**

Use a space after (most) keywords. The notable exceptions are sizeof, typeof, alignof, and \_\_attribute\_\_.

So use a space after these keywords:

    if, switch, case, for, do, while

but not with sizeof, typeof, alignof, or \_\_attribute\_\_. E.g.,

    s = sizeof(struct file);
Do not add spaces around (inside) parenthesized expressions.

When declaring pointer data or a function that returns a pointer type, the preferred use of * is adjacent to the data name or function name and not adjacent to the type name. Examples:
```
char *linux_banner;
unsigned long long memparse(char *ptr, char **retptr);
char *match_strdup(substring_t *s);
```
Use one space around (on each side of) most binary and ternary operators, such as any of these:

    =  +  -  <  >  *  /  %  |  &  ^  <=  >=  ==  !=  ?  :
but no space after unary operators:

    &  *  +  -  ~  !  sizeof  typeof  alignof  __attribute__  defined
No space before the prefix or postfix increment & decrement unary operators:

    ++  --
and no space around the . and -> structure member operators.
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

We try to limit lines to 120 characters before breaking the text to the second line. We make exceptiosn for legacy translated texts and block of text that makes sense to be presented on one line (e.g., a bash command that we cannot split on two lines without reducing readability) 

3) **Placing braces and spaces**

Put the opening brace last on the line, and put the closing brace first:
```
if (condition) {
    action();
}
```

Do not unnecessarily use braces where a single statement will do.
```
if (condition)
    action();
```
and
```
if (condition)
    do_this();
else
    do_that();
```

This does not apply if only one branch of a conditional statement is a single statement; in the latter case use braces in both branches:

```
if (condition) {
    do_this();
    do_that();
} else {
    otherwise();
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




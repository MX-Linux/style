# Style Guide for MX apps 

The main purpose of using a specific style is to standardize the code to increase readability and avoid confusions.

Main principle: we should always choose clarity over saving keystrokes.

## Qt/C++ Applications
We are mostly following https://www.kernel.org/doc/html/v4.10/process/coding-style.html with a handful of exceptions.
Where there is doubt about any style in this guide, please refer to the Linux Kernel style guide above.

Where there are conflicts between the MX Linux style guide and the Linux kernel style guide, the MX Linux style guide always takes precedence over the Linux kernel style guide.

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
Lines of this nature are not to exceed 70 characters.
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

4) **Variable Declarations**

4.1) **Maintaining minimal scope**
Where possible, declare variables as late as possible, with the minimal amount of scope. This makes it clearer where they are used, such that removal becomes easier.
```
int foo;
foo = do_something(1);
int bar = do_another_thing(foo);
```

However, consider aesthetics and readability. If many variables are being used, it may be better to declare them all in the same block of lines:
```
int foo, bar;
foo = do_something(1);
bar = doo_another_thing(foo);
```

4.2) **Initialization**

Prefer initialization to late initial assignment where possible:
```
int foo = do_something(1);
int bar = do_another_thing(foo);
```

A variable must have a value assigned to it within 10 lines after its declaration:
```
int foo;
do_something(1);
do_something_else(2);
do_something2(3);
another_function(4);
test_call(5);
test_call(6);
another_test(7);
// Blank lines and comments are counted within the 10-line range.
if(test(9)) foo = 9;
else foo = 10;
```

If this cannot be achieved, a variable must be initialized to a suitable default value:
```
int foo = 0;
do_something(1);
do_something_else(2);
do_something2(3);
another_function(4);
test_call(5);
test_call(6);
another_test(7);
yet_another_test(8);

if(test(10)) foo = 10;
else foo = 11; // Outside the 10-line limit.
```


4.2) **Use of the "const" keyword**

Always declare a variable as "const" if it is never modified beyond its initial assignment.

```
const int foo = 5;
const int bar = foo * test;
```

Note that pointers are treated differently with the const keyword:
```
const char *str = "hello"; // Constant content, but "str" can be assigned to something else.
char *const str = "hello"; // Content can be modified, pointer variable cannot be re-assigned.
const char *const str = "hello"; // Both content and pointer variable are constant.
```

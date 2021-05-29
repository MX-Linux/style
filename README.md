# Style Guide for MX Linux Applications
The main purpose of using a specific style is to standardize the code to increase readability and avoid confusions.

Main principle: we should always choose clarity over saving keystrokes.

## Qt/C++ Applications
We are mostly following the [Linux kernel coding style](https://www.kernel.org/doc/html/v4.10/process/coding-style.html) with a handful of exceptions.
Where there is doubt about any style in this guide, please refer to the Linux Kernel style guide above.

Where there are conflicts between the MX Linux style guide and the Linux kernel style guide, the MX Linux style guide always takes precedence over the Linux kernel style guide.

### 1. Indentation
We use 4 blank spaces for each level of indentation.

Don’t put multiple statements on a single line unless you have something to hide:
```
if (condition) do_this;
    do_something_everytime;
```
Don’t put multiple assignments on a single line either. 

### 2. Breaking long lines and strings
Try to limit lines to 100 characters at most. Lines that need to exceed 100 characters must not exceed 120 characters.
We make exceptions for legacy translated texts and block of text that makes sense to be presented on one line (e.g., a bash command that we cannot split on two lines without reducing readability).

All whitespace (such as indentation) and comment text is considered as part of the line, and counts towards the line length.
For the purposes of this calculation, a single tab character is considered to occupy 4 characters of a line.

#### 2.1. Breaking conditional statements into multiple lines
For very short statements in conditionals, it is acceptable to have a compact format where the statement is on the same line as the conditional:
```
if (conditional) do_this();

if (condition1) do_this();
else if (condition2) do_this();
else if (condition3) do_this();
else do_something_else();
```
Lines of this nature are not to exceed 70 characters.
If the line contains a trailing comment, the code before the comment cannot exceed 70 characters. The whole line cannot exceed 100 characters.

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

#### 2.2. Breaking loops into multiple lines
A *for* or *while* loop may have a compact format, providing the line does not exceed 70 characters:
```
for (int ixi = 0; ixi < 100; ++ixi) do_something(ixi);
while (test == true) test = do_stuff();
```
If the line contains a trailing comment, the code before the comment cannot exceed 70 characters. The whole line cannot exceed 100 characters.

A *do ... while* loop is not to have a compact form such as above, regardless of line length:
```
do {
    something(condition);
} while (condition);
```

### 3. Placing braces and spaces
Put the opening brace last on the line, and put the closing brace first:
```
if (condition) {
    action();
}
```

#### 3.1. Spaces
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

### 4. Variable Declarations
Any variable, function, or object, must be used at least once within its applicable scope.

#### 4.1. Maintaining minimal scope
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

Counting loops should have the variable declaration and initialization in the loop body wherever feasible:
```
for (int ixi = 0; ixi < 123; ++ixi) {
    do_something_repetitively(ixi);
}
```

Keeping in mind the 120-character hard limit (see section 2), a loop counter may be declared before the loop body if the initialization will not fit:
```
int ixLoop = super_duper_long_variable_initialization_function(1) * where_the_loop_starts(1);
for (; ixLoop < 123; ++ixLoop) {
    do_something_repetitively(ixLoop);
}
```

#### 4.2. Initialization
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
if (test(9)) foo = 9;
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

if (test(10)) foo = 10;
else foo = 11; // Outside the 10-line limit.
```


#### 4.3. Use of the "const" keyword
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

### 5. Naming convention for variables and objects
Much of the naming convention is similar to the Linux kernel guide, with some exceptions.

Some discretion may be used when naming variables, this is a guide to naming variables.
However, objects such as Qt GUI elements, must follow a specified pattern.

#### 5.1. Function names
Function names must be descriptive, and "camel-case" notation is to be used.
```
void function()
{
    test();
}
void functionSomething()
{
    testAnotherThing();
}
```
Parameter names should be short, and to the point.

#### 5.2. General-purpose variables
Short names such as "tmp" are preferred, which is much easier to write, and not the least more difficult to understand.

Mixed-case names are not frowned upon like the kernel guide, however splitting a piece of code into multiple functions should be considered if a variable name needs to be overly descriptive.

Local variable names should be short, and to the point.
A loop counter may be called i. Calling it loop_counter is non-productive, if there is no chance of it being mis-understood.
Similarly, tmp can be just about any type of variable that is used to hold a temporary value.

If your variables need to be more descriptive, consider splitting the code into multiple functions.

#### 5.3. C++ Classes
Custom class types must start follow a similar convention to camel-case, however the fist letter must be upper-case.
```
class TestClass
{
    int variable;
    int classVar;
public:
    TestClass();
    ~TestClass();
}
```

#### 5.4. Qt GUI Elements defined in UI Files
Qt objects as defined by UI files (eg. created by Qt Designer or Qt Creator) must be in camel-case and start with a prefix denoting the type of object, followed a descriptive name of what the object does.
The prefixes are as follows:

|Element(s)                         |Prefix|Example
|-----------------------------------|------|-------
|Labels                             |label |labelUser
|Group Boxes, Frames                |box   |boxSettings
|Tabs (within a QTabWidget)         |tab   |tabServer
|Text fields                        |text  |textUser
|Spin boxes                         |spin  |spinPartSize
|Combo boxes                        |combo |comboType
|Buttons (push/command/tool)        |button|buttonOK
|Radio buttons                      |radio |radioProfileNew
|Check boxes                        |check |checkUserActive
|Lists (QListView/QListWidget)      |list  |listObjects
|Trees (QTreeView/QTreeWidget)      |tree  |treeHierarchy
|Tables (QTableView/QTableWidget)   |table |tableFileData

For Qt elements not in the above list, prefixes are not necessary, but use some common sense with names.

This does not apply to local variables (section 5.2) used as temporary Qt objects.

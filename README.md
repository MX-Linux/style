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
Try to limit lines of this nature to 70 characters at most. Lines of this nature that need to exceed 70 characters are not to exceed 100 characters. These limits include any trailing comments.

If any conditional check has braces, all conditionals within the if...else if...else structure must also have braces, regardless of line length:
```
if (condition1) {
    do_this();
} else if (condition2) {
    do_this();
} else if (condition3) {
    do_something_else_that_has_a_very_long_name();
} else {
    do_something_else();
}
```

#### 2.2. Loops
A loop is not to have a compact form such as above, regardless of line length:
```
// Non-compliant loops
for (int ixi = 0; ixi < 100; ++ixi) do_something(ixi);
while (test == true) test = do_stuff();

// Compliant loops
for (int ixi = 0; ixi < 100; ++ixi) {
    do_something(ixi);
}
while (test == true) {
    test = do_stuff();
}
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
// Blank lines and comments are counted within the 10-line range.
if (test(8)) {
    foo = 9;
} else {
    foo = 11; // not allowed, over 10 lines beyond declaration.
}
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

if (test(10)) {
    foo = 11; // Outside the 10-line limit, but already initialized.
} else {
    foo = 13;
}
```


#### 4.3. Use of the "const" and "constexpr" keywords.
Always declare a variable as "const" if it is never modified beyond its initial assignment. Use "constexpr" if the variable is derived from an expression that can be definitively evaluated at compile time.

```
constexpr int foo = 5;
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
|Container of tabs (QTabWidget)     |tabs  |tabsSettings
|Tabs (within a QTabWidget)         |tab   |tabServer
|Text fields                        |text  |textUser
|Spin boxes                         |spin  |spinPartSize
|Combo boxes                        |combo |comboType
|Buttons (push/command/tool)        |push  |pushOK
|Radio buttons                      |radio |radioProfileNew
|Check boxes                        |check |checkUserActive
|Lists (QListView/QListWidget)      |list  |listObjects
|Trees (QTreeView/QTreeWidget)      |tree  |treeHierarchy
|Tables (QTableView/QTableWidget)   |table |tableFileData
|Progress bars and such indicators  |prog  |progSetup

For Qt elements not in the above list, prefixes are not necessary, but use some common sense with names.

This does not apply to local variables (section 5.2) used as temporary Qt objects.

### 6. Slots and Signals
Wherever practical, use the [newer Qt syntax for signals](https://wiki.qt.io/New_Signal_Slot_Syntax):
```
connect(sender, &Sender::valueChanged, receiver, &Receiver::updateValue);
```
If it is necessary to use the old method of connecting by name, a comment explaining the reason should be included.

#### 6.1. Avoid the signal/slot facility of Qt Designer UI files
Whilst it can be tempting to use this feature of Qt Designer, it is not recommended. Using this facility is error-prone, and breaks the separation between the form design and the code.
It can make debugging code very difficult, as the behaviour of the GUI is not visible in the code.

Instead, connect the slots to signals in code after setting up the UI.
```
connect(ui->pushClose, &QPushButton::clicked, this, &MainWindow::close);
connect(ui->checkBoot, &QCheckBox::toggled, ui->frameBoot, &QFrame::setEnable);
```
Keeping slot connections in code separates the GUI design and presentation from the code.
# Spacemacs conventions

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc/generate-toc again -->
**Table of Contents**

- [Spacemacs conventions](#spacemacs-conventions)
    - [Code guidelines](#code-guidelines)
        - [Spacemacs core and layer](#spacemacs-core-and-layer)
        - [All layers](#all-layers)
    - [Key bindings conventions](#key-bindings-conventions)
        - [Reserved prefix](#reserved-prefix)
            - [User prefix](#user-prefix)
            - [Major mode prefix](#major-mode-prefix)
        - [Evilify buffers](#evilify-buffers)
        - [Navigation](#navigation)
            - [n and N](#n-and-n)
            - [Code Navigation](#code-navigation)
            - [`insert state` buffers](#insert-state-buffers)
        - [Evaluation](#evaluation)
        - [REPLs](#repls)
            - [Send code](#send-code)
            - [In terminal](#in-terminal)
        - [Building and Compilation](#building-and-compilation)
        - [Debugging](#debugging)
        - [Tests](#tests)
            - [All languages](#all-languages)
            - [Language specific](#language-specific)
        - [Refactoring](#refactoring)
        - [Help or Documentation](#help-or-documentation)

<!-- markdown-toc end -->

## Code guidelines

### Spacemacs core and layer

Function names follow these conventions:
- `spacemacs/xxx` is an interactive function called `xxx`
- `spacemacs//xxx` is a private function called `xxx` (implementation details)
- `spacemacs|xxx` is a _macro_ called `xxx`

Variables follow these conventions:
- `spacemacs-xxx` is a variable
- `spacemacs--xxx` is a private variable (implementation details)

### All layers

A package is initialized in a function with name `<layer>/init-xxx` where:
- `<layer>` is the layer name
- `xxx` is the package name

## Key bindings conventions

### Reserved prefix

#### User prefix

<kbd>SPC o</kbd> must not be used by any layer. It is reserved for the user.

#### Major mode prefix

<kbd>SPC m</kbd> is reserved for the current major mode. Three keys bindings
are not an issue (ie. <kbd>SPC m h d</kbd>) since <kbd>SPC m</kbd> can be
accessed via <kbd>,</kbd>.

### Evilify buffers

`Spacemacs` offers convenient functions to _evilify_ a buffer.
_Evilifying_ a buffer is to:
- add `hjkl` navigation
- add incremental search with `/`, `n` and `N`
- add `visual state` and `visual line state`
- add yank (copy) with `y`
- activate evil-leader key
- fix all bindings shadows by the above additions

To fix the shadowed bindings we capitalize them, for instance:
shadowed `h` is transposed to `H`, if `H` is taken then it is
transposed to `C-h` and so on...

Example of _evilified_ buffers are `magit status`, `paradox buffer`.

The related functions are:
- `spacemacs/activate-evil-leader-for-maps` and `spacemacs/activate-evil-leader-for-map`
- `spacemacs/evilify`

### Navigation

#### n and N

To be consistent with the Vim way, <kbd>n</kbd> and <kbd>N</kbd> are favored
over Emacs <kbd>n</kbd> and <kbd>p</kbd>.

Ideally a micro-state should be provided to smooth the navigation experience.
A micro-state allows to repeat key bindings without entering each time the
prefix commands.
More info on micro-states in the [documentation](DOCUMENTATION.md#micro-states).

#### Code Navigation

The prefix for going to something is `<SPC> m g`.

    Key           |                 Description
------------------|------------------------------------------------------------
<kbd>m g a</kbd>  | go to alternate file (i.e. `.h <--> .cpp`)
<kbd>m g g</kbd>  | go to things under point
<kbd>m g t</kbd>  | go to corresponding test file if any

#### `insert state` buffers

Navigation in buffers like `Helm` and `ido` which are in `insert state` should
be performed with <kbd>C-j</kbd> and <kbd>C-k</kbd> bindings for vertical
movements.

    Key         |                 Description
----------------|------------------------------------------------------------
<kbd>C-j</kbd>  | go down
<kbd>C-k</kbd>  | go up

### Evaluation

Live evaluation of code is under the prefix `<SPC> m e`.

    Key           |                 Description
------------------|------------------------------------------------------------
<kbd>m e $</kbd>  | put the point at the end of the line and evaluate
<kbd>m e b</kbd>  | evaluate buffer
<kbd>m e e</kbd>  | evaluate last expression
<kbd>m e f</kbd>  | evaluate function
<kbd>m e l</kbd>  | evaluate line
<kbd>m e r</kbd>  | evaluate region

### REPLs

#### Send code

A lot of languages can interact with a REPL. To help keeping a consistent
behavior between those languages the following conventions should be
followed:
- `<SPC> m s` is the prefix for sending code. This allows fast
interaction with the REPL whenever it is possible
- lower case key bindings keep the focus on the current buffer
- upper case key bindings move the focus to the REPL buffer

    Key           |                 Description
------------------|------------------------------------------------------------
<kbd>m s b</kbd>  | send buffer
<kbd>m s B</kbd>  | send buffer and switch to REPL
<kbd>m s d</kbd>  | first key to send buffer and switch to REPL to debug (step)
<kbd>m s D</kbd>  | second key to send buffer and switch to REPL to debug (step)
<kbd>m s f</kbd>  | send function
<kbd>m s F</kbd>  | send function and switch to REPL
<kbd>m s i</kbd>  | start/switch to REPL inferior process
<kbd>m s l</kbd>  | send line
<kbd>m s L</kbd>  | send line and switch to REPL
<kbd>m s r</kbd>  | send region
<kbd>m s R</kbd>  | send region and switch to REPL

Note: we don't distinguish between the file and the buffer.

#### In terminal

History navigation in shells or REPLs buffers should be bound as well to
<kbd>C-j</kbd> and <kbd>C-k</kbd>.

    Key         |                 Description
----------------|------------------------------------------------------------
<kbd>C-j</kbd>  | next item in history
<kbd>C-k</kbd>  | previous item in history
<kbd>C-l</kbd>  | clear screen
<kbd>C-r</kbd>  | search backward in history

### Building and Compilation

The base prefix for major mode specific compilation is <kbd>SPC m c</kbd>.

    Key Binding      |                 Description
---------------------|------------------------------------------------------------
<kbd>m c b</kbd>     | compile buffer
<kbd>m c c</kbd>     | compile
<kbd>m c r</kbd>     | clean and compile

Note: we don't distinguish between the file and the buffer. We can implement
an auto-save of the buffer before compiling the buffer.

### Debugging

The base prefix for debugging commands is <kbd>SPC d</kbd>.

    Key Binding      |                 Description
---------------------|------------------------------------------------------------
<kbd>m d a</kbd>     | abandon current process
<kbd>m d b</kbd>     | toggle a breakpoint
<kbd>m d B</kbd>     | clear all breakpoints
<kbd>m d c</kbd>     | continue
<kbd>m d d</kbd>     | start debug session
<kbd>m d i</kbd>     | inspect value at point
<kbd>m d l</kbd>     | local variables
<kbd>m d n</kbd>     | next
<kbd>m d r</kbd>     | run
<kbd>m d s</kbd>     | step

Notes:
- Ideally a micro-state for breakpoint navigation should be provided.
- If there is no toggle breakpoint function, then it should be implemented at
the spacemacs level and ideally the function should be proposed as a patch
upstream (major mode repository).

### Tests

A lot of languages have their own test frameworks. These frameworks share
common actions that we can unite under the same key bindings:
- `<SPC> m t` is the prefix for test execution.
- `<SPC> m T` is the prefix for test execution in debug mode (if supported).

#### All languages

    Key           |                 Description
------------------|------------------------------------------------------------
<kbd>m t a</kbd>  | execute all the tests of the current project
<kbd>m t b</kbd>  | execute all the tests of the current buffer
<kbd>m t t</kbd>  | execute the current test (thing at point, function)

Note: we don't distinguish between the file and the buffer. We can implement
an auto-save of the buffer before executing the tests of buffer.

#### Language specific

    Key           |                 Description
------------------|------------------------------------------------------------
<kbd>m t m</kbd>  | execute the tests of the current module
<kbd>m t s</kbd>  | execute the tests of the current suite

Note that there are overlaps, depending on the language we will choose one
or more bindings for the same thing

### Refactoring

Refactoring prefix is <kbd>SPC m r</kbd>.

### Help or Documentation

The base prefix for help commands is <kbd>SPC h</kbd>. Documentation is
considered as an help command.

    Key           |                 Description
------------------|------------------------------------------------------------
<kbd>m h h</kbd>  | documentation of thing under point
<kbd>m h r</kbd>  | documentation of selected region

# Spec of Cablin

## Notation

The **code** is specified using the YAML.

## Identifier

An identifier can contain any char **in the current version**. However, there will be more restrictions in the future version.

## Value and Type

Cablin provides 5 types:

* 32-bits Integer
* 64-bits Integer
* Float
* Boolean
* String

## Package

A package will be a sequence of package commands.

A package command is one of the types shown below:

- definition of package-variables
- definition of functions
- import command

```yaml
- <function def/var def/import>
- <function def/var def/import>
- <function def/var def/import>
# ...
```

### Import

Import a user package or **builtin package**

```yaml
import: <package name>
```

### Function Definition

Function definition requires:

- `name`: An identifier to be its name.
- `params`: List of parameters
- `body`: Function's body a sequence of commands. When calling this function,
  the interpreter will execute commands sequentially.

```yaml
func:
  name: <function name>
  params: # list of parameter
    - name: <variable name 1>
      type: <type name 1>
    - name: <variable name 2>
      type: <type name 2>
      # ...
  body: # list of command
    - <command_1>
    - <command_2>
    # ...
```

## Expression

The interpreter will calculate the expression and get a value.
Here are the types of expression.

### Get variable

Get a variable's value. 

```yaml
get: <variable name>
```

### Constant

Use constant straightforwardly.

```yaml
const:
  type: <type name>
  value: <value>
```

### Unary Operator

Apply unary operator on an expression, including:

| Unary Operator Name | What                                           |
|:-------------------:|:-----------------------------------------------|
|        `not`        | Only for boolean: True -> False, False -> True |
|        `neg`        | Only for Numeric Value: x -> -x                |

```yaml
<unary operator name>:
  <expr>
```

### Binary Operator

Use binary operator on two **values which types are the same**.
The interpreter will throw an exception when types are not the same.

| Binary Operator Name | int    | int64  | float  | string | bool |
|:--------------------:|:------:|:------:|:------:|:------:|:----:|
|        `plus`        |  +     |   +    |   +    | concat |      |
|        `minus`       |  -     |   -    |   -    |        |      |
|       `multiply`     |$\times$|$\times$|$\times$|        |      |
|        `divide`      |  /     |   /    |   /    |        |      |
|         `mod`        |  %     |   %    |   %    |        |      |
|         `and`        |        |        |        |        |  and |
|         `or`         |        |        |        |        |  or  |
|        `equal`       | $=$    | $=$    | $=$    | $=$    | $=$  |
|      `not_equal`     |$\neq$  | $\neq$ | $\neq$ | $\neq$ |$\neq$|
|      `greater`       |$>$     | $>$    | $>$    | $>$ (lexical)   |      |
|      `less`          |$<$     | $<$    | $<$    | $<$ (lexical)   |      |
|      `greater_equal` |$\geq$  | $\geq$ | $\geq$ | $\geq$ (lexical)|      |
|      `less_equal`    |$\leq$  | $\leq$ | $\leq$ | $\leq$ (lexical)|      |


```yaml
<binary operator name>:
  - <expr_1>
  - <expr_2>
```

### Function call

Use the result of the function call.

```yaml
<same as function call command>
```

## Command

### Variable Definition

To define a variable, we need:

- `name`: A identifier for its name
- `type`: Name of its type
- `default_value`: Default value

When this command occurs in package scope, it will define a package variable (global variable), which is visible to all the functions.

When this command is used in a function, it will define a local variable.

```yaml
var:
  name: <variable name>
  type: <type name>
  default_value: <value>
```

### Variable Assignment

The variable assignment will assign the calculated result of source expression to the target variable.

```yaml
assign:
  target: <variable name>
  source:
    <expr>
```

### Function Call

The function call will call a function by calculating the result of a list of expressions.

```yaml
call:
  name: <function name>
  params: # list of expresssion
    - <expression_1>
    - <expression_2>
    # ...
```

### If

**If** calculate the result of the expression is `true`, the interpreter will execute the command sequence in `then` block, otherwise `else` block.

```yaml
if:
  condition:
    <expression>
  then: # list of command
    - <command_1>
    - <command_2>
    # ...
  else: # list of command
    - <command_1>
    - <command_2>
    # ...
```

### While

The While command provides loop-logic.

Before entering a while loop, the interpreter will calculate the condition. If it's true, the interpreter will execute the command sequence in the `body` block, otherwise no execution.

The interpreter will execute the `body` block again and again until the condition becomes false.

```yaml
while:
  condition:
    <expression>
  body: # list of command
    - <command_1>
    - <command_2>
    # ...
```

### Countinue/Break

**Continue** and **Break** are flow control commands for the While command. Both of them will affect the nearest While command.

**Continue** will jump to the next cycle, While **Break** will jump out of the While command.

```yaml
continue:
break:
```

### Block

Create a scope for variables.

```yaml
block:
  - <command_1>
  - <command_2>
  # ...
```

### Return

Return the calculated result of an expression in a function.

```yaml
return:
  <expression>
```
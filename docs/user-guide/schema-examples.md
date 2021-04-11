# Examples

## If branching

If-branch logic can be comprehended as the c++ code:

```cpp
if (condition) {
   // ...
}
```

The code shown below will execute the `then` part if the variable `c` is true.
The `then` part only contains a function calling command which calls the print function to print variable `a`.

```yaml
- if:
    condition:
      get: c
    then:
      - call:
          name: io::print
          params:
            - get: a
```

The structure of `else` is similar.

```yaml
- if:
    condition:
      get: c
    then:
      - call:
          name: io::print
          params:
            - get: a
    else:
      - call:
          name: io::print
          params:
            - get: b
```

## Define a Variable

In Cablin, a variable should be **defined** before using it.

* Define an integer variable which name is `a` and the default value is `1`.

```yaml
- var:
    type: int
    name: a
    default_value: 1
```

* Define a string variable which name is `b` and the default value is `Hello string`

```yaml
- var:
    type: string
    name: b
    default_value: Hello string
```

## Assign a Variable with a Value

* `a = a + b`:

The Cablin interpreter doesn't support infix expression currently.
Instead, it provides a `plus` node. Input two values, and return their sum.

Hence, in the `source`, we use the `plus` node and get `a + b`. 

```yaml
- assign:
    target: a
    source:
      plus:
        - get: a
        - get: b
```

## Scope

When an expression tries to fetch the value of a variable.
The interpreter will try to find in the most nearby scope.

```yaml
- block:
    - var:
        name: a
        type: int
        default_value: 1
    - if:
        # The condition is a constant: true. Hence it will be always passed.
        condition:
            const:
                type: bool
                value: true

        # then is also a scope
        then:
            - var:
                name: a
                type: int
                deafult_value: 2
            - call:
                name: io::print
                params:
                    - get: a
```

The result of the print command is `2`.

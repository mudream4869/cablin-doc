# Cablin UI Demo

[Cablin UI Demo](https://mudream4869.github.io/cablin-ui/)

![ui screen snapshot](./img/cablin-ui-screenshot.png)

## What's this

The interpreter run in this web page is built by Emscripten.
For more detail, please visit the Github repo:
[cablin-wasm](https://github.com/mudream4869/cablin-wasm).

## Cablin Wasm

This js version only provide two functions:

```cpp
// Run Cablin script with an output function
// Return 1 when an exception occurred
// Return 0 when succeed
int cablinRun(const char* body, void(*print)(const char*));

// getCablinError return ex.what()
const char* getCablinError();
```

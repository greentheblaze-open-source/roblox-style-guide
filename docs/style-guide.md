# Luau Style Guide

## Type Checking

Type checking is useful for identifying bugs in code. Here we recommend ways to utilize it where necessary (where inferred types are either unable to be identified, or are misleading).

### Type Aliases

Type aliases are useful when creating either unique blueprints for objects, or indicating something could be more than one type. We use them only where necessary.

1. Do not ever assign type aliases to a single generic type.

    ```luau { .style-good-auto-header }
    type AnyCharacter = Character | PlayerCharacter
    ```

    ```luau { .style-bad-auto-header }
    type Success = boolean

    local success: Success = pcall(someFunction)
    ```

    ```luau { .style-exception-auto-header }
    -- It's cleaner to define them under a name rather than having to reference it through the module itself:
    type Character = Types.Character
    ```

#### Defining Functional Types

When defining functions as types (not to be confused with [Type Functions][type-functions]), you'll see syntax like the following...

```luau
type SomeFunction = (parameterA, parameterB) -> ()
```

...where the the first set of brackets defines the parameters a function will have, and the last set defines a tuple for output values from that function.

1. If there is only 1 output value in a tuple, exclude the brackets - there's no need for them.

    ```luau { .style-good-auto-header }
    type CheckAlive = (character: Model) -> boolean
    ```

    ```luau { .style-bad-auto-header }
    type CheckAlive = (character: Model) -> (boolean)
    ```

### Type Annotations

1. Always annotate declared variables.
1. Never annotate variables that are initialized with a literal value.
1. If a function in the same run context is said to return a specific type, do not type annotate the variable(s) that is to hold the result of the function when called.
1. Use annotations when the result is not instantly obvious from a variable's initialization.

### Type Casting

### Generics

1. All substitutions must be descriptive of what's being expected.

    ```luau { .style-good-auto-header }
    type Table<Key, Value> = { [Key]: Value }
    type Dictionary<Value> = Table<string, Value>
    ```

    ```luau { .style-bad-auto-header }
    type Table<K, V> = { [K]: V }
    type Dictionary<V> = Table<string, V>
    ```

## Tables

### Lists

1. Put a space after an opening parenthesis and before a closing parenthesis.

    ```luau { .style-good-auto-header }
    local people = { "Alex", "Fred", "Charlie" }
    ```

    ```luau { .style-bad-auto-header }
    local people = {"Alex", "Fred", "Charlie"}
    ```

### Dictionaries & Objects

1. No index-like keys.

    ```luau { .style-good-auto-header }
    local hungerLevels = {
        George = 10,
        Mary = 12,
        Fred = 20,
    }
    local oldestPerson = { name = "George" }
    ```

    ```luau { .style-bad-auto-header }
    local personA = {
        ["name"] = "Fred",
    }
    local personB = { ["name"] = "George" }
    ```

    ```luau { .style-exception-auto-header }
    -- This should be a rare circumstance to ever need to do this though:
    local oddPersonA = {
        ["Mythically-Rare Name"] = "Fred",
    }
    ```

1. Group related keys.

    ```luau { .style-good }
    local tiers = {
        fred = "Low",
        george = "Low",

        amy = "Medium",

        charlie = "High",
    }
    ```

## Comments

### Disabling Lines of Code

1. Use single-line comments.

    ```luau { .style-good-auto-header }
    -- local personA = {
    --    name = "Fred",
    -- }
    -- local personB = { name = "George" }
    ```

    ```luau { .style-bad-auto-header }
    --[[
        local personA = {
        name = "Fred",
        }
        local personB = { name = "George" }
    ]]

    --[=[
        local personA = {
        name = "Fred",
        }
        local personB = { name = "George" }
    ]=]
    ```

[type-functions]: https://luau.org/types/type-functions/

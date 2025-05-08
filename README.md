# What are Some Differences Between Interfaces and Types in TypeScript?

TypeScript provides two main ways to describe the shape and structure of data: `interface` and `type`. At first glance, they appear to be interchangeable, and in many cases they are. However, there are key differences between the two that can significantly affect how you structure your code and organize your application.

In this blog post, we’ll explore the important distinctions between `interface` and `type`, when to use each, and best practices for clean and scalable TypeScript code.

---

## Similarities at a Glance

Before diving into the differences, it’s important to understand that both `interface` and `type` can be used to define object shapes.

```ts
interface User {
  name: string;
  age: number;
}

type User = {
  name: string;
  age: number;
};
````

For basic use cases like this, the choice is often a matter of preference. But there are notable distinctions once you move into more advanced use cases.

---

## 1. Extending and Inheritance

### Interface

Interfaces are designed to be extended. They support a clear syntax for inheritance.

```ts
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
}
```

### Type Alias

Type aliases can also be extended, but they do so through intersection types.

```ts
type Person = {
  name: string;
};

type Employee = Person & {
  employeeId: number;
};
```

**Conclusion**: Both support inheritance, but interfaces have more intuitive syntax for extending and are often preferred for this use case.

---

## 2. Declaration Merging

### Interface

One of the unique capabilities of interfaces is declaration merging. If you declare an interface with the same name more than once, TypeScript will merge them.

```ts
interface Animal {
  species: string;
}

interface Animal {
  age: number;
}

// Resulting type: { species: string; age: number }
```

### Type Alias

Type aliases do not support declaration merging.

```ts
type Animal = {
  species: string;
};

type Animal = {
  age: number;
}; // Error: Duplicate identifier 'Animal'
```

**Conclusion**: Only interfaces can merge declarations, making them useful in scenarios like extending third-party libraries or frameworks.

---

## 3. Complex Type Definitions

Type aliases are more flexible when defining complex types such as unions, tuples, and conditional types.

```ts
type ID = string | number;

type Callback = () => void;

type Position = [number, number];
```

Interfaces cannot directly express these kinds of types.

**Conclusion**: Use type aliases when working with unions, intersections, function signatures, or tuples.

---

## 4. Use in Class Implementations

Both interfaces and types can be used to describe the shape of objects that classes implement, but interfaces are generally preferred in this case due to better compatibility and readability.

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message);
  }
}
```

---

## 5. Tooling and Readability

In many development environments, especially with modern editors like Visual Studio Code, interfaces tend to offer better readability and editor support. This includes better documentation, clearer auto-complete, and improved hover information.

---

## When Should You Use Each?

| Use Case                                  | Recommended |
| ----------------------------------------- | ----------- |
| Object shape and class definitions        | Interface   |
| Extending other types or interfaces       | Interface   |
| Declaration merging needs                 | Interface   |
| Defining union, intersection, tuple types | Type Alias  |
| Function types or complex constructs      | Type Alias  |

---

## Final Thoughts

Both `interface` and `type` are essential features of TypeScript. While they may seem similar, understanding their differences will help you make informed choices and write more maintainable and scalable code.

A common best practice is:

* Use `interface` for public APIs, object shapes, and extensible contracts.
* Use `type` for more complex type definitions and union-based logic.

Choosing the right one depends on your specific use case, but using them appropriately will improve the clarity, safety, and quality of your TypeScript codebase.

---

# Understanding `keyof` in TypeScript with Examples

One of TypeScript's most powerful and underrated features is the `keyof` keyword. It plays a crucial role in creating **type-safe, dynamic, and reusable code**, especially when working with object properties.

In this blog post, we will explore what `keyof` does, why it's useful, and walk through a practical example to solidify your understanding.

---

## What is `keyof`?

The `keyof` keyword in TypeScript is a **type operator** that returns a **union of string literal types** representing all the keys of an object type.

In simpler terms, if you give it an object type, it gives you a type that consists of **only the property names** of that object.

---

## Basic Example

```ts
type User = {
  id: number;
  name: string;
  isActive: boolean;
};

type UserKeys = keyof User;
// Equivalent to: 'id' | 'name' | 'isActive'
````

Here, `UserKeys` becomes a union type of all the keys in the `User` object.

---

## Why is `keyof` Useful?

The primary benefit of `keyof` is that it allows you to **write generic functions** that are type-safe and can access object properties dynamically **without losing type information**.

---

## Practical Example: Dynamic Property Access

Suppose you want to write a function that takes an object and a key and returns the value corresponding to that key. Here's how `keyof` helps:

```ts
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

// Usage
const user = {
  id: 101,
  name: 'Alice',
  isActive: true
};

const name = getValue(user, 'name'); // Type is string
const activeStatus = getValue(user, 'isActive'); // Type is boolean
```

This is fully type-safe:

* If you try to pass a key that doesn’t exist in the object, TypeScript will show a compile-time error.
* The return type is inferred correctly based on the key.

---

## Common Use Cases of `keyof`

* Creating dynamic form inputs or configurations
* Type-safe data access in utility libraries
* Advanced generic types and mappers
* Building custom hooks or components in React with strict typing

---

## Conclusion

The `keyof` keyword in TypeScript empowers developers to write highly flexible, generic, and type-safe code. By constraining property keys to known values, it minimizes bugs and improves code predictability.

Understanding how to use `keyof` effectively is a fundamental step toward mastering advanced TypeScript patterns.
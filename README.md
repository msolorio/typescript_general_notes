# TypeScript - General Notes

#### as const
assigns all properties of an object to literal types that cannot be changed.

```ts
const person = {
  firstName: 'Frank',
  lastName: 'Herbert'
} as const;
```

#### strictNullChecks
Setting to true in tsconfig.json gives distinct types to null and undefined. Gives an error if they occur when a concrete value is expected.
- Best to set this to true

#### Type Guard
A conditional that confirms the type of value so that assumptions can then be made on using that value.


Without a type guard
```ts
function padLeft(padding: number | string, input: string) {
  return " ".repeat(padding) + input; // gives a type error because padding could be a string.
}
```

With a Type Guard
```ts
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
```

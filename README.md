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

// Use in operator to check the type
function printDocument(doc: DeliminatedDocument | PlaintextDocument) {
  if ("separator" in doc) {
    // treat as deliminated doc
  } else {
    // treat as plain text doc
  }
}
```

### Type Predicate
Defining a function wich returns true if parameter is a particular type. Resets the type of the parameter.
```js
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}
// If function returns true, pet is assigned to type 'Fish'

if (isFish(pet)) {
  pet.swim();
} else {
  pet.fly(); // TypeScript now knows we have type 'Bird'
}
```

Readonly object properties
```ts
interface Todo {
  readonly title: string
  readonly description: string
}

const todo1: Todo = {
  title: 'todo 1',
  description: 'some text'
}

todo1.title = 'other title'; // TypeScript guards against.
```

#### Utility Types
https://www.typescriptlang.org/docs/handbook/utility-types.html


#### Readonly
Can create readonly utility types of an existing interface
https://www.typescriptlang.org/docs/handbook/utility-types.html

```ts
interface Todo {
  title: string
  body: string
}

type ReadOnlyTodo = Readonly<Todo>

// interface ReadOnlyTodo: Readonly<Todo>
// Not allowed. How to create an interface from another.
```

Record type - an object type allowing for specifyings types for keys and values.
```ts
interface CatInfo {
  age: number;
  breed: string;
}
 
type CatName = "miffy" | "boris" | "mordred";
 
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

#### NonNullable
Constructs a type excluding null and undefined.

```ts
type PersonName = string | null | undefined

type ExistingPersonName = NonNullable<PersonName>

const name1: ExistingPersonName = null; // Type 'null' is not assignable to type 'string'.

const name2: ExistingPersonName = 'Frank';
```

### Lookup Types
Allow for defining types from nested members of an object.

```ts
type Route = {
  origin: {
    city: string
    state: string
    departureFee: number
  }
  destination: {
    city: string
    state: string
    arrivalFee: number
  }
}

type Origin = Route['origin']
type Destination = Route['destination']

const origin1: Origin = {
  city: 'Denver',
  state: 'CO',
  departureFee: 5 
}
```

### Generics
Allows for creating types and functions that can be specified for parameters.
- aids reusability.

https://www.typescriptlang.org/docs/handbook/2/generics.html

When function parameters are specified as generic, TypeScripts expects that they could be any type.

```ts
// identity function will take in any param and return that param.

// The same parameterized type is used as the input and the return type.
function identity<Type>(arg: Type): Type {
  return arg;
}

// calling the function
let output = identity<String>('my string');

// Can also use type inference to infer type of 'Type' in simple cases.
let output = identity('my string');

// The identity function itself has this type.
let identityFnx: <Type>(arg: Type) => Type = identity;

//**************************************************************************
// Can Set functions to work on a generic array
// Now we can use .length prop on arg.
function loggingIdentity<Type>(arg: Type[]): Type[] {
  console.log(arg.length);
  
  return arg;
}

//**************************************************************************
// Can specify Type to be any type that also has a length property
interface HasLength {
  length: number
}

function loggingIdentity<Type extends HasLength>(args: Type) {
  console.log(arg.length);

  return arg;
}

loggingIdentity(3); // throws a type error.

//**************************************************************************

// Can use generics to define an interface
interface GenericData<Type> {
  data: Type
  owner: string
  department: string
}

const numberData: GenericData<number[]> = {
  data: [12, 1, 3, 4],
  owner: 'Sally',
  department: 'HR'
}
```

### KeyOf Type Operator

Creates a union type of the keys of an object.

```ts
interface Point {
  x: number,
  y: number
}

type P = keyof Point;

let prop: P = 'x';

prop = 'y';

// prop = 'z'; // 'z' is not assignable to type keyof Point;
```

### TypeScript's typeof

```ts
let s = 'hello';
let n: typeof s
```

```ts
function f() {
  return { x: 10, y: 3 };
}

type P = ReturnType<typeof f>
```

### Conditional Types
come back to this.

https://www.typescriptlang.org/docs/handbook/2/conditional-types.html


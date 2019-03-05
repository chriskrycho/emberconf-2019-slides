----

#### Structural typing

TypeScript has a *structural* type system.

- Not like C++, Java, C♯
- More like OCaml/Reason, F♯, Elm

(Don’t panic! It’s okay if you don’t have experience with *any* of those.)

Note: TypeScript is *not* like the type systems most people have experience with from C++, Java, C♯, etc. It *is* like the type systems of OCaml (including Reason), F♯, Elm, Haskell, etc. but most people’s idea of a type system comes from Java or C♯, and the differences can be very surprising.

---- 

##### Structural typing: the most important thing

*Types are just shapes.*

```ts
interface Person {
  firstName: string;
  lastName: string;
}












```

Note: In a structural type system, types are *just shapes*. Anything with that shape can be used wherever the shape is required. So in this example, we have a `Person` shape – we’ll talk more about using `interface` to define shapes later.

---- 

##### Structural typing: the most important thing

*Types are just shapes.*

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}








```

Note: Then we can define a function which takes a `Person`.

---- 

##### Structural typing: the most important thing

*Types are just shapes.*

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}

const person = {
  firstName: 'Chris',
  lastName: 'Krycho',
  favoriteHobby: 'running',
};


```

Note: Then we can create an object which *doesn’t* explicitly call itself a `Person` but which *does* match the shape.

---- 

##### Structural typing: the most important thing

*Types are just shapes.*

```ts
interface Person {
  firstName: string;
  lastName: string;
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}

const person = {
  firstName: 'Chris',
  lastName: 'Krycho',
  favoriteHobby: 'running',
};

sayHello(person);
```

Note: But we can use that with the `sayHello` function, because it has the right shape – even though it has *more* properties than needed, and even though it doesn’t have the *name* that we used to define that shape.

Again: shapes are the only thing TypeScript cares about! And this is a huge difference from C++ or Java or C♯, which all care whether the *name* of the thing you’re using matches.

---- 

##### Structural typing: `class`

- *Names don’t matter.*
- A `class` is just a way to *define* and *construct* a shape.

```ts
class Person {
  firstName: string;
  lastName: string;

  constructor(first: string, last: string) {
    this.firstName = first;
    this.lastName = last;
  }
}

function sayHello(p: Person) {
  console.log(`Hello, ${p.firstName} ${p.lastName}!`);
}

const me = { firstName: "Chris", lastName: "Krycho", employer: "LinkedIn" };
sayHello(me); // "Hello, Chris Krycho!"
```

Note: Building on that: *names of types don’t matter* in TypeScript. When you write a `class` definition, you’re just providing a definition of a shape, and a way to build that shape, all at once. So you can see here a function that takes a `Person`, and a `Person` is defined with a `class` – but TypeScript doesn’t care whether you constructed the shape using a class constructor an object literal. It only cares that you pass it something that matches the *shape* you defined.

---- 

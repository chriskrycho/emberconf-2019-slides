### What *is* TypeScript?

- *Basically* a typed superset of JavaScript
- *Strictly* a compile-to-JavaScript language
- But close enough that we can think of it as "JS with types"

Note: TypeScript is *basically* a typed superset of JavaScript. I say *basically* because there are a few constructs in TypeScript which don’t exist in JavaScript. We’ll talk about those in a few minutes, but the fact that those exist means TypeScript is *strictly speaking* a distinct language which compiles to JavaScript. For most purposes, though, it’s fine to think of it as a superset of JavaScript with types.

---- 

#### Cool, but why should I care?

Two big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors
3. Confident refactoring

…*and* it’s not painful to use!

Note: So that’s all well and good, but *why should you care?* Maybe that’s interesting if you’re (like me) kind of weirdly obsessed with type systems. But what does it gain you as JavaScript developer every day? How does it make your life easier?

1. How many of you here like having docs for your functions? Now, how many of you would like it if those docs were always *right* and *up to date*? Well, the first thing about TypeScript is that that’s exactly what it gives you. My experience of using TypeScript is *not*, for the most part, the way I’ve felt in some other programming languages, where I’m writing down names of things just because. It’s more like just documenting “for this function to work *at all*, it needs you to pass in a thing that has *this property* on it”—and then finding out *in my editor* if I passed in the wrong thing, or if my function doesn’t return what the docs say it does. So that’s handy.

2. The second thing that makes TypeScript *really great* is that it legitimately helps us ship fewer “undefined is not an object” kinds of errors to production. And I care about that not in the abstract sense but because every single one of those I ship to production means *something didn’t work for a user*. It also means it’s time I have to spend hunting down the cause of that bug instead of building something new—whether that new thing is adding a feature, or making the app work offline, or building a whole new product, or whatever else. TypeScript doesn’t get the count to zero, like some programming languages can—but it helps, a *lot*.

3. The biggest developer experience bit is the one I've saved for last: you can refactor with *confidence*. In my experience, refactors in any significantly large JavaScript codebase are extremely difficult: it's really, really hard to know when you've actually caught *everything* impacted by a change. TypeScript lets you just start making changes and keep following the compiler's messages until you have updated *every* place affected. The first time you do that and build your project and *everything just works* feels amazing.

Finally, it’s worth note that it’s not *painful to use* in the way some typed languages have been. If I need to write “This function needs an object with a `quack` method on it that I can call” I can just write that inline, and we’ll see that in a few minutes! The types *cost* a lot less than they do in the sort of “typical” typed languages out there, which makes their relative value a lot higher, too.

---- 

### What *is* TypeScript?

A great way to super-charge your library and application development!

Note: In short, TypeScript is not just a "typed superset of JavaScript": it's a way to develop faster and more reliably, *especially* when you have to make changes! So let's dig in.

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

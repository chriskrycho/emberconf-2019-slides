---

### [fit] Types
#### [fit] What even are they?

---

### Types
#### [fit] Types in JavaScript

- Primitive types: `boolean`, `string`, `number`, `symbol`
- Objects: `{ name: "Chris" }`
- Arrays: `[1, 2, 3]`
- Functions
- Classes

^There are four “primitive” types in JavaScript: strings, booleans, numbers, and symbols. There are also *objects* and *arrays*, and in their own special category, there are *classes*.

---

### Types
#### [fit] Types in TypeScript

- Primitive types: `boolean`, `string`, `number`, `symbol`
- Objects: `{ name: "Chris" }`
- Arrays: `[1, 2, 3]`
- Functions
- Classes

…and a few more!

^TypeScript has the *same* sets of types (and a couple more we'll see in a minute).

---

### Types

<br/>

#### [fit] Compile-time and Run-time

JavaScript and TypeScript both have types.

The difference is *when those types matter*!

---

### Types
#### Compile-time and Run-time

JavaScript types exist at *runtime*:

```js
let me = {
  name: "Chris Krycho"
};

me.greet();  // RUNTIME ERROR
```

> TypeError: foo is not a function

---

### Types
#### Compile-time and Run-time

TypeScript types exist at *compile time*:

```ts
let me = {
  name: "Chris Krycho"
};

me.greet();  // BUILD TIME ERROR
```

> Property 'greet' does not exist on type '{ name: string; }'.

---

### Types

<br/>

#### [fit] Type Signatures

How do I tell TypeScript what types these things are, anyway?

^So let’s talk about how we actually write down types to use in TypeScript. That’ll give us the foundation we need for using them in the context of Ember.js specifically.

---

### Types
#### Type Signatures

variable name + `:` + name of type

```ts
let variableName: ItsType = /* its value */;
//              ╰───────╯
//          The type signature
```

---

### Types
#### Type Signatures

Booleans:

```ts
let tsIsCool: boolean = true;
//          ╰───────╯
//     The type signature
```

---

### Types
#### Type Signatures

Numbers:

```ts
let age: number = 1;
//     ╰──────╯
// The type signature
```

---

### Types
#### Type Signatures

Strings:

```ts
let name: string = "Chris";
//      ╰──────╯
// The type signature
```

---

### Types
#### Type Signatures

Symbols:

```ts
let theAnswer: Symbol = Symbol(42);
//           ╰──────╯
//      The type signature
```

---

### Types
#### Type Signatures

Objects:

```ts
let person: { name: string } = { name: "Chris" };
//        ╰────────────────╯
//        The type signature
```

^Object types look like object literals, but with *types* instead of *values* after the name of the field.

---

### Types
#### Type Signatures

Arrays:

```ts
let languages: string[] = ["TS", "Rust"];
//           ╰────────╯
//       The type signature
```

---

### Types
#### Type Signatures

Arrays again:

```ts
let languages: Array<string> = ["TS", "Rust"];
//           ╰─────────────╯
//         The type signature
```

(This is a *generic* type; we'll come back to this.)

^Array types can be written two ways: as `Array<{the type, like "number" here}>` or `{the type, like "number" here}` followed by `[]`. We’ll come back to the version with `Array` written out explicitly in a few.

---

### Types
#### Type Signatures

Functions:

```ts
function length(s: string): number {
//               ╰──────╯ ╰──────╯
//              The type signatures
  return s.length;
}
```

^For functions, we can write the type of the inputs and the outputs.

---

### Types
#### Type Signatures

Arrow functions:

```ts
let length = (s: string): number => s.length;
//             ╰──────╯ ╰──────╯
//            The type signatures
```

^This works for arrow functions, too!

---

### Types
#### Type Signatures

Classes:

```ts
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
```

^We define a class just like we do in JavaScript, but annotate its *members*.

---

### Types

<br>

#### [fit] Type Inference

You often don't have to do anything!

---

### Types
#### Type Inference

```ts
let myName: string = "Chris";
let theAnswer: number = 42;
let aLie: boolean = false;


function length(s: string): number {
  return s.length;
}


let length = (s: string): number => s.length;
```

---

### Types
#### Type Inference

```ts
let myName = "Chris";
let theAnswer = 42;
let aLie = false;


function length(s: string) {
  return s.length;
}


let length = (s: string) => s.length
```

---

### Types
#### Type Inference

```ts
let myName = "Chris";  // automatically `string`
let theAnswer = 42;    // automatically `number`
let aLie = false;      // automatically `boolean`

// automatically returns `number`
function length(s: string) {
  return s.length;
}

// automatically returns string
let length = (s: string) => s.length
```

^A lot of times, you *won’t* have to write down types. Anywhere you assign a value, TypeScript *infers* the type for you. Similarly, TypeScript will figure out function return types for you.

---

### Types
#### Type Inference
##### [fit] TypeScript's type inference is pretty smart!

```ts
// returns `string | number`
function moreComplicated(itIs: boolean) {
  return itIs ? 'yay' : 42;
}

let a = moreComplicated(true);
let b = a + 2;
//      ╰───╯ Type error!
```

> Operator '+' cannot be applied to types
> 'string | number' and '2'

^TypeScript can infer a *lot*. It’ll even infer more interesting types we haven’t talked about yet, like *union* types. Here, TypeScript knows that we’re returning *either* a string or an object with a key named “say” which has a string value. And when we go to use it, we’ll have to check what the type is, or TS will yell at us.

---

### Types
#### Type Inference

<br/>

##### [fit] But (unlike some languages) it is not *total*.

Sometimes we still have to write down types. 😢

^But (unlike some other programming languages) it can’t infer *everything*. Another way to say this is that it isn't *total*. There are interesting reasons for this, but for our purposes it's enough to know that sometimes we still have to write type annotations.

---

### Types
#### Type Inference
##### [fit] Limitations: **Arrays**

This will not go well:

```ts
let aBunchOfThings = [];
//  ╰───any[]────╯
```

Add a type annotation:

```ts
let aBunchOfThings: number[] = [];
```

^For example, if you create an empty array, you’ll need to tell it what *kind* of array, or it’ll fall back to the `any` type.

---

### Types
#### Type Inference
##### [fit] Limitations: **Functions**

This might blow up at runtime:

```ts
function whatIsThis(breakfast) {
  //                ╰──any──╯
  return breakfast.wafflesPlease; // this might 💥 at runtime?
}
```

---

### Types
#### Type Inference
##### [fit] Limitations: **Functions**

Write it out:

```ts
function whatIsThis(breakfast: { wafflesPlease: boolean }) {
  //                ╰─────────*not* any anymore!────────╯
  return breakfast.wafflesPlease; // safe!
}
```

^You also have to write function *argument* types explicitly pretty much all the time; TS doesn’t try to do total program inference like some other languages do.

^That goes double when using *generic* functions (which we'll talk more about in a minute), like `_.map` from lodash.

---

### Types
#### Optional values

<br>

##### [fit] What about `null` and `undefined`**?**

---

### [fit] Aside: _**strictness**_

- TypeScript can be more or less “strict.”
- Configure it in `tsconfig.json` (with lots of other options)
- `strict: true` is your friend!
    - best balance of help to effort

^TypeScript can check your code more or less strictly. You can set this in fine-grained detail in your `tsconfig.json`, which is home to *all* the configuration for your project. I recommend just using `strict: true`, though, which turns on *all* the strictness settings.

^You might wonder about that: wouldn't it be easier to just start with the lightest set of changes and then slowly dial up the strictness. My personal experience says *no*: you just end up having to cover the same ground over and over and over again, which is exhausting. Even more importantly, though, `strict: true`is where TypeScript is *helping you out most*. At lower strictness, you still have to do most of the same work to annotate your code—those limitations we just saw are still in play!—but you don't get *most* of the help.

^Now, opinions vary on that, even among the ember-cli-typescript core team! But that's my recommendation.

---

### Types
#### Optional values
##### [fit] *__Everything__* is optional with low strictness.

```ts
let myName: string = null;
console.log(myName.length); // 💥 at runtime
```

☝🏼 That’s totally valid! 😢

^If you turn off TypeScript's strictness settings around optionals, *everything* is optional. Your declarations basically mean “Whatever I say this is, but also `null` or `undefined`.” This means we get no help with our great nemesis: `undefined is not a function`.

---

### Types
#### Optional values

TypeScript can help! In `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strictNullChecks": true
  }
}
```

^TypeScript *can* help us with this, if we set `strictNullChecks` (or, better, for all the reasons I outlined above), `strict` to `true` in the `compilerOptions` in `tsconfig.json`.

---

### Types
#### Optional values
##### [fit] Mark types as optional with **`?`**

```ts
let myName: string = null;
//       type error!─╯

let maybeMyName?: string = null;
//             ╰───okay!───╯

```

^Once we have that switch on, we have to annotate optional types with a `?`. Otherwise, types are *not* nullable! We can only assign `null` or `undefined` to them if they are marked with that question mark.

---

### Types
#### Classes

Just like classes in JavaScript… with type annotations!

```ts
class Person {
  age: number;
  name?: string;
  
  constructor(age, name?: string) {
    this.age = age;
    this.name = name;
  }
}
```

---

<!-- TODO: keep working from here -->

### Types

<br>

#### [fit] `any`

`any` is the great (and *terrible*) escape hatch.

---

### Types
#### `any`

```ts
function madWithPower(noLimits: any) {
  return noLimits.noHelpEither.ohNo;
}
  
madWithPower("just a string");  // 💥 at runtime
```

> TypeError: undefined is not an object
>   (evaluating 'noLimits.noHelpEither.ohNo')

^TypeScript gives us an escape hatch, and I’ll tell you about it up front because it’s a useful tool while you’re converting your codebase, and for *rare* occasions after that. But it’s also *dangerous*.

^The `any` type is exactly what it sounds like. You’re telling TypeScript, “This can be anything, and don’t check me on *anything* I do with it.” Worse, it *infects* its context. Once you use it for a given item, TS basically throws away everything it knows about that item.

^That means TypeScript *cannot help you* with anything marked as being of the type `any`. No autocompletion. No type errors. Nothing. `any` is an escape hatch, but once you *do* get your types written down, you should use it as a tool of last resort and be *very* careful with runtime checks when you do bust it out.

---

### Types
#### `any`

If you *must* use `any`… give your colleagues some help!

```ts
// As of TypeScript 3.4, the compiler doesn’t understand that
// `somethingFine` is equivalent with the type `Neato`. See
// <https://github.com/Microsoft/TypeScript/issues/#####> for
// details.
let somethingWeird = (somethingFine as any) as Neato;
```

^I strongly recommend that you *never* use `any` except in very specific scenarios where it the TypeScript compiler has already fallen down for some reason—and then leave a detailed comment explaining *why* you had to do that.

^Now, what about the times when a function legitimately *can* handle literally *any* input you throw at it? For that, TypeScript has a different tool: `unknown`.

---

#### `unknown`

<!-- TODO: fill in the example -->

```ts
function handleInput(input: unknown): SanitizedInput {
}
```

^Instead of `any`, most of the time you want to use `unknown`. `unknown` is like `any` in that TS doesn’t know what it is, but instead of letting you do *whatever you want* with it, TypeScript won’t let you do *anything* with it unless you explicitly do the work to figure out what type it actually is.

^ This ends up being what we actually want most of the time: we don’t know what this thing we got handed was, but we can do a little work at runtime to make handling it safe! And as we’ll talk about in a bit, TS is clever enough to understand the implications of these kinds of runtime checks!

---

#### Writing shapes

Since TS is all about shapes, let’s write some!

- `type` for “type aliases”
- `interface` for “extensible shapes”
- `class` for JS classes with annotations

Note: Since TypeScript is all about shapes, how do we write them

---

##### Writing shapes: `type`

  function withGnarly(arg: {
    a: string[];
    b: number;
    c: { some: boolean };
  }): boolean { /* ... */ }
  
  type Arg = {
    a: string[];
    b: number;
    c: { some: boolean };
  };
  
  function withGnarly(arg: Arg): boolean { /* ... */ }

Note: A type alias is a way of telling TypeScript “When I use this name, it’s just a shorthand for this shape!” We can write shapes inline, but that gets nasty quickly. So we can create an alias for them and use that instead.

---

##### Writing shapes: `interface`

  interface Name {
    primary: string;
  }
  
  interface WesternName extends Name {
    middle?: string;
    last: string;
  }


Note: Another way to write a shape is with an `interface`, which can be *extended* and *implemented*. These are basically interchangeable with type aliases, except for those two differences. Here, the `WesternName` has all the properties of `Name` and adds in an optional `middle` and required `last` name. You can use these wherever you’d use a type alias… but also with classes!

---

##### Writing shapes: `class`

TypeScript classes are just JavaScript classes with type annotations.

Note: A TypeScript class is *basically* just a JavaScript class with type annotations for all the bits attached to it.

---

##### Writing shapes: `class`

(Remember: they also define shapes!)

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
  
  const bill = { firstName: "Bill", lastName: "Pullen", employer: "Olo" };
  sayHello(bill); // "Hello, Bill Pullen!

Note: But it’s also worth remembering that a `class` declaration *also* defines a shape in TypeScript. So you can also think of it as being a way to define a shape and a way to *build an instance* of the shape at the same time.

---

##### Writing shapes: `extends` and `implements`

  class Person implements Name {
    first: string;
    age: number;
  }
  
  class Programmer extends Person {
    knownJSFrameworks: string[];
  }

Note: classes can also implement interfaces and extend other classes. If they declare they implement an interface, TypeScript will check that everything the interface has, the class has! Meanwhile `extends` works just like it does in normal JavaScript: you attach to the prototype of the type you’re extending.

---

##### Writing shapes: when to use each

- `type` as the default
- `interface` for defining shapes for more than one `class` to conform to
- `class` for a convenient way to get a shape and a constructor at the same time

Note: My basic tack is I start with a `type` alias, and rarely go beyond that. That’s where you really get the “this is just documentation my editor helps me check!” approach. I switch to an `interface` only if I’m going to define multiple `class`es that need to implement a certain shape contract. And I use `class` pretty rarely *other* than when I’m building Ember components or services or whatever. (My own code is mostly just functions, and `type` aliases work *great* with functions!)

I should note: I’m offering an opinionated take here. This actually runs up against Microsoft’s view a little bit – they basically say to use interfaces for everything and not to use type aliases at all, because things should *always* be open to being extended. I disagree! I often want to just write down a bunch of small types like LEGO blocks to fit together. But there’s room for different styles here, in any case.

---

#### “nullable” types: getting a handle on `null` and `undefined`!

- the “optional” annotation
- strict null checking

Note: Next, one of the most important things TypeScript can help us with—I would argue, at least!—is `undefined` and `null`. How many people in this room have seen “undefined is not an object” or similar in the last week?  Same. We’re slowly eradicating them from our app, but… it’s taking a while.

TypeScript gives us two tools we can combine to help us fix this problem: *optional* type annotations, and the *strict null checking* compiler option.

---

##### “nullable” types: the “optional” annotation

  function mightConcat(a: string, b?: string) {
    return b ? a + ' ' + b : a;
  }
  
  type Name = {
    primary: string;
    surname?: string;
    other?: string[];
  }

Note: The optional type annotation is just a question mark, applied to optional function arguments or optional properties on an object type.

Here, for example, we have a function which joins two strings... but only if the second string is supplied; otherwise it just returns the first string. The question mark tells TypeScript that it’s legitimate to leave off the second argument.

Likewise, if we were trying to build up a not-so-Western-focused version of a *person* type, we might say that everyone has an age, and that there are also optional combinations of kinds of names. To build a person, we always need an `age` value, but we can make a name with TODO *neither*, *either*, or *both* of the other keys.

---

##### “nullable” types: strict null checking

Turn on `"strictNullChecking": true` in `tsconfig.json`!

  let el: HTMLElement = document.querySelector('some-id');
  el.focus(); // Type error! This could be `null`!

Note: We can combine optional declarations with the `"strictNullChecking"` compiler option in our `tsconfig.json` file, which is where *all* the compiler options go. We’ll look at that file a bit in the second session. For now, it’s just important to know that if we turn on `strictNullChecking`, anywhere that *could* be `null` or `undefined`, TypeScript will require us to check for it! This can be a little annoying, but it means fewer bugs in production.
  
If you’re starting a *new* Ember app with TypeScript, I’d turn this flag on at the start. If you’re dealing with an existing app... well, that’s probably going to be too hard, but it’s worth aiming to get there eventually!

---

#### Generics

  let numbers = [1, 2, 3]; // Array<number>
  let strings = ['a', 'b', 'c']; // Array<string>
  let things = [
    { thing: 1 },
    { thing: 2 },
  ]; // Array<{thing: number }>

Note: I’m not going to spend a *lot* of time on generic types, although they’re both very *important* and very *powerful*. We’ll see some examples of them in the refactoring section, and I’ll talk about them in more detail then. However, I think it’s worth introducing them so you recognize the syntax, and talking a *little* about how to use them.

Generics let us capture things like the fact that we can have an array of just about anything: arrays of numbers, of strings, of objects, etc. If we want to be able write that down, especially for new types *we* build, we need a syntax for it, to tell the compiler what we mean. That’s what generics are.

You can see in the example here: an array can be an array of all sorts of things – numbers, strings, complex objects, etc. If we build up our *own* containers that can hold more than one kind of thing, we can do that with generic types. I don’t expect to cover this much today, but you’ll *see* it, so it’s helpful to know what the notation means!

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

^For example, if you create an empty array, you’ll need to tell it what *kind* of array, or it’ll fall back to the `any` type. We'll talk more about `any` in a while—it is *not* your friend, in general—but for the moment you just need to understand that you can shove *anything* into this array.

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

^The same thing goes for functions: here, `breakfast` is implicitly of that `any` type, so you can pass anything into this function. It'll… probably blow up at runtime.

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

### Types
#### [fit] Aside: _**strictness**_ in TypeScript

- TypeScript can be more or less “strict.”
- Configure it in `tsconfig.json` (with lots of other options)
- `"strict": true` is your friend!
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
#### Optional values
##### [fit] A **real-world** example

TypeScript can save our bacon with DOM APIs!

```ts
let el: HTMLElement = document.querySelector('#some-id');
console.log(el.innerHTML.length);
//          ╰─could be null!──╯
```

^This isn't just a hypothetical, either. Many DOM APIs, for example, return `null` if they don't find the thing you looked for. In that case, attempting to act on the element they return will blow up at runtime if the element doesn't exist.

^Expect a question here!

---

### Types
#### Classes

```ts
class Person {
  age: number;
  name?: string;
  
  constructor(age: number, name?: string) {
    this.age = age;
    this.name = name;
  }

  greet() {
    let greeting = this.name ? `Hi, I'm ${this.name}!` : "Hey!"
    console.log(`${greeting} I'm ${this.age} years old`);
  }
}
```

^Classes in TypeScript are just like ES6 classes in JavaScript… with type annotations! That means they're real runtime entities you can pass around, too, just as in plain JavaScript.

^I'm not going to dive into the details of classes today, as they're not TypeScript specific. They're just syntax sugar for normal prototypal inheritance. The point *here* is that they let you describe the shape of the class: the types that can live on it.

^You write these types just like you do types elsewhere.

^As a quick aside: if you're wondering why this `Person` has an optional `name`, well… look up Patrick McKenzie's blog post [Falsehoods Programmers Believe About Names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) when you're done here. Suffice it to say: there are lots of cultural-linguistic contexts where there's *no* single name which is always right to call a person!

^Now, back to talking about classes!

---

### Types
#### Classes
##### Initializers

```ts
class Person {
  age = 0;
  name?: string;

  constructor(age?: number, name?: string) {
    if (age) {
      this.age = age;
    }

    this.name = name;
  }
}
```

^We also have *field initializer* syntax: we can give a given field a default value. As usual, TypeScript will correctly infer the type here if we give it a value. (Also, this now works with Ember! If you checked things out any time in 2017 or 2018, that wasn't true, but we collaborated with the Ember core team because it matters for both JavaScript and TypeScript—and they fixed it as of Ember 3.6, with a polyfill back to 3.4!)

---

### Types
#### Classes
##### Initializers (cont'd.)

```ts
class Person {
  age = 0;
  name?: string; // INITIALIZED TO `undefined`

  constructor(age?: number, name?: string) {
    if (age) {
      this.age = age;
    }

    this.name = name;
  }
}
```

^An important thing to note: declaring a type *initializes its value to `undefined`* if you're using Babel to compile your TypeScript. That's because the specification for ES6 classes says defining an item on a type (without a type annotation, of course!) does that. This has performance benefits for the JS runtimes. If you compile with TypeScript's own compiler, that's *not* true (though hopefully it will change to match the spec in the future). We'll talk about how both Babel and TypeScript compilers are used in ember-cli-typescript in a little bit.

---

### Types
#### What about regular objects?

Classes exist at *runtime*.

What if we want to describe a type only at *compile time*?

```ts
let me = {
  name: "Chris Krycho",
  age: 31,
}
```

---

### Types
#### What about regular objects?

You can write it inline:

```ts
let me: {
  age: number;
  name?: string;
} = {
  age: 31,
  name: "Chris Krycho"
};
```

---

### Types
#### What about regular objects?

Inline type annotations for objects are:

- *verbose*: you have to spell them out every time!
- *repetitive*: you can’t reuse them!

^Inline type annotations for objects are *verbose*: you have to spell them out inline! It's not easy to follow this code. And as a result, they're also *repetitive*: you can’t reuse them! You have to write this same thing out everywhere you want this type—in variable declarations, functions, etc.

^Inline type annotations are *just fine*… sometimes. But a lot of times, we want something better.

---

### Types

TypeScript has *two* tools to help us here.

#### [fit] **`type`** (type aliases)
#### [fit] **`interface`** (types)

^Happily, TypeScript has us covered here, in two different ways (which have their own tradeoffs). One uses the `type` keyword to define a type *alias*; and the other uses the `interface` to do the same to define a *type*.

---

### Types
#### **`type`** (type aliases)

Type aliases are just *reusable names*:

```ts
type Person = {
  age: number;
  name?: string;
};

let me: Person = {
  age: 31,
  name?: "Chris Krycho",
};
```

^So: type aliases. Type aliases are just *reusable names* the compiler can substitute for the inline form. So, here for example, I've extracted that inline definition of a person to a *name*, `Person`, which I can then use wherever I would have used the inline form. This has no runtime existence. Type aliases are just a way to give a name to a particular shape for reuse and brevity.

---

### Types
#### **`type`** (type aliases)

Type aliases are just *reusable names*…

…which are the only good way to do:

- union and sum types
- mapped and lookup types
- conditional types

^TypeScript's type aliases are *just* reusable names… but that's actually their superpower, because it lets you give a name to a number of TypeScript's most useful and sophisticated types: union and sum types, mapped and lookup types, and conditional types. We'll briefly talk about some of these in a few minutes. For now, it's just worth noting that type aliases can do these things, and interfaces *can't*.

---

### Types
#### **`interface`** (types)

Interfaces are a lot like type aliases:

```ts
interface Person {
  age: number;
  name?: string;
}

let me: Person = {
  age: 31,
  name?: "Chris Krycho",
};
```

^Now, let's talk about interfaces, though, because they *can* do some really neat things of their own. First, interfaces are a lot like type aliases for the basics. This is an interface definition which is *equivalent* to the type alias we just saw.

---

### Types
#### **`interface`** (types)

Interfaces are a lot like type aliases…

…*but* with different abilities:

- they can `extend` each other
- `class`es can `implement` them

---

### Types
#### **`interface`** (types)
#####  Extension

Interfaces can *extend* other interfaces:

```ts
interface Person {
  age: number;
  name?: string;
}

interface Developer extends Person {
  programmingLanguages: string[];
}
```

^So, here, `Developer extends Person`: this means that any `Developer` instance must always have an age and optionally a name, and it must also have a list of programming languages they know. Could be empty, could be a dozen long… but it has to be there.

^Type aliases can *not* do this. (There is a comparable thing you can do which we'll cover *very* briefly at the end of this first session, but they cannot do `extend` like this.)

---

### Types
#### **`interface`** (types)
#####  With `class`

Classes can *implement* interfaces:

```ts
class Writer implements Person {
  age: number;
  name?: string;
  humanLanguages: string[];

  constructor(age: number, name: string, languages: string[]) {
   this.age = age;
   this.name = name;
   this.humanLanguages = pls;
  }
}
```

^Next up: classes can *implement* interfaces. Here, we can see a `Writer` class which implements `Person`—it looks a lot like our `Developer` type from a minute ago, but here we have a constructor, and we're back to having an item which actually exists at runtime.

<!--NOTE: this probably has to be skipped for time. It's also a bit arcane.

---

### Types
#### **`interface`** (type aliases)

Type aliases are *eagerly resolved*.
Interfaces are *lazily resolved*.

Implications for:

- tooling: type name vs. type contents
- performance (rarely): lazy helps sometimes

^NOTE: CAN SKIP IF SHORT ON TIME.

^There's one other really important difference. Type aliases are *eagerly resolved*, while interfaces are *lazily resolved*. In other words, the compiler just substitutes the definition of the type alias anywhere it finds it *immediately*—but for interfaces, it keeps the name around, and doesn't figure out

^This means that for *tools*, like the VS Code hover, you'll see all the details of a type alias; but you'll just see the *name* for aliases.

^It can also sometimes have performance implications for the compiler. This doesn't come up often, but sometimes the lazy evaluation for interfaces is helpful.

END SKIPPABLE-->

---

### Types

<br>

#### [fit] Types are just **shapes**!

- *does* feel like duck typing
- does *not* feel like C++, Java, C♯, etc.

^Now, I need to pause and talk through something incredibly important about TypeScript here: in TypeScript, _types are **just shapes**_.

^For those of you coming from JavaScript, this is what makes TypeScript feel relatively natural. It means the *type system* is an awful lot like duck typing: anything that matches a particular shape works!

^For those of you coming from typed languages like C++, Java, C♯, etc.—or even something like F♯ or Rust—this will *not* feel like those!

---

### Types
#### Types are just shapes!

```ts
interface Person {
  age: number;
  name?: string;
}
function describe(person: Person) {/*...*/}

type Human = {
  age: number;
  name?: string;
};

let me: Human = {
  age: 31,
  name: "Chris"
};
describe(me);
```

^To make this a lot clearer, let's see how it works in actual code.

^Interfaces in TypeScript are a lot like interfaces in languages like Java or C♯, with an important distinction: in those languages, two interfaces with the same shape but different names are *different types*. In TypeScript, they're totally compatible.

^This example is a little dense, so let's walk through it closely. On the first line, we have a type alias which defines a `Person` shape to substitute in, and then a `describe` function which uses that type alias. Then we have an `interface` which defines a `Human` type, with the same properties on it as the `Person` type alias, and then I create an object which I specify to be of type `Human`. Finally, we call the `describe` function with the `me` object—and even though those types have different names, this works without issue.

---

### Types
#### Types are just shapes!

```ts
interface Person {
  age: number;
  name?: string;
}
function describe(person: Person) {/*...*/}

describe({
  age: 31,
  name: "Chris Krycho"
});

let me = {
  age: 31,
  name: "Chris",
};
describe(me);
```

^Because we just care about shapes we can create an object which *doesn’t* explicitly call itself a `Person` but which *does* match the shape. We can do this anonymously inline by just passing an object, or with a standalone POJO. Notice that neither of them has any type annotation. That's okay! TypeScript only cares that the shapes line up.

---

### Types
#### Types are just shapes!

```ts
interface Person {
  age: number;
  name?: string;
}
function describe(person: Person) {/*...*/}

let meWithExtraDetails = {
  age: 31,
  name: "Chris Krycho",
  favoriteHobby: "running",
  degrees: [
    "B.S. in Physics",
    "M. Div."
  ],
};
describe(meWithExtraDetails);
```

^TypeScript also doesn't care if the shape you use in a given spot has *additional* properties. It just has to have the minimal set the type requires. So here, I have a POJO that describes me again, but with a bunch of *extra* details, like my hobby and the degrees I have (I'm, uhh, kind of a nerd it turns out). This is just fine, because the minimal shape is included.

---

### Types
#### Types are just shapes!

```ts
interface Person {
  age: number;
  name?: string;
}
function describe(person: Person) {/*...*/}

class Human {
  age: number;
  name?: string;
  constructor(age: number, name?: string) {
    this.age = age;
    this.number = number;
  }
}
let me = new Human(31, "Chris Krycho");
describe(me);
```

^We could even use a class here!

---

### Types
#### Types are just shapes!

```ts
class Person {
  age = 0;
  name?: string;
}
function describe(person: Person) {/*...*/}

let fromClass = new Person();
fromClass.age = 31;
fromClass.name = "Chris Krycho";
describe(fromClass);

let notFromClass = {
  age: 31,
  name: "Chris Krycho",
};
describe(notFromClass);
```

^An important point here: the *shape* of the class is the only thing that matters. That includes classes, even though they exist at runtime, too: they're just a way to *construct* a given shape.

---

### Types
#### Types are just **shapes**!

<br>

##### [fit] “Structural types”

Names *don't matter*.
Only shapes do.

^This system—where types are *just shapes*, and their names *don't matter*, only the shapes do—is called *structural typing*. It takes a while to get your head around, but it's *incredibly* powerful once you do. It's what makes TypeScript feel like a safe version of duck typing.

^Expect a question here about downsides. Value of nominal typing: needing a way to be able to "tag" variants, e.g., and having to do it manually.

---

### Types
#### Deciding between `type`, `interface`, `class`

- `type` for naming *local* types (in apps/addons/libraries)
- `interface` for:
    - public interfaces in libraries
    - defining how classes should behave
- `class` for a convenient way to get a shape and a constructor at the same time

^My basic tack is I start with a type alias in app code or private code for a library, and I rarely go beyond that. That’s where you really get the “this is just documentation my editor helps me check!” approach. I switch to an `interface` only if I’m writing library code I expect to be extended in some way, or where I'm going to define multiple classes that need to implement a certain shape contract. And I use `class` pretty rarely *other* than when I’m building Ember components or services or whatever. (My own code is mostly just functions, and `type` aliases work *great* with functions!)

^I should note: I’m offering an opinionated take here. This actually runs up against Microsoft’s view a little bit – they basically say to use interfaces for everything and not to use type aliases at all, because things should *always* be open to being extended. I disagree! I often want to just write down a bunch of small types like LEGO blocks to fit together. But there’s room for different styles here, in any case.

---

### Types

<br>

#### [fit] Dealing with ignorance:
#### [fit] **`any`** and
#### [fit] **`unknown`**

^Now, what about the times when we don't know what's coming into our program? TypeScript gives us two options for dealing with this: `any` and `unknown`.

---

### Types
#### Dealing with ignorance

<br>

##### [fit] `any`

`any` lets us do *anything*.
It effectively *disables the type-checker*.
We don’t want that!

^The older of these two, and the more dangerous, is `any`. It lets us do *literally anything*. I'm not exaggerating: it effectively disables the type-checker wherever you put it. And… trust me, you don’t want that. Let's see why.

---

### Types
#### Dealing with ignorance
##### `any`

```ts
let noLimits: any = "just a string";
console.log(noLimits.noHelpEither.ohNo.sob);
```

> TypeError: undefined is not an object
>   (evaluating 'noLimits.noHelpEither.ohNo.sob')

^Here we can see what happens if we 

^If you use `any`, TypeScript *cannot help you*. No autocompletion. No type errors. Nothing. Using `any` is kind of like using TS in its least strict modes, but even worse than those. It means you end up doing a lot of the *work* to use TypeScript, but with little of the benefit.

^`any` has always been dangerous, but it used to be necessary for certain things, like untrusted data coming in from outside your system. Today, however, there is only and exactly one place you should use `any`, because we have `unknown`, as of TypeScript 3.0.

---

### Types
#### `any`

If you *must* use `any`… give your colleagues some help!

```ts
// As of TypeScript M.N, the compiler doesn’t understand that
// `somethingFine` is equivalent with the type `Neato`. See
// <https://github.com/Microsoft/TypeScript/issues/#####> for
// details.
let somethingWeird = (somethingFine as any) as Neato;
```

^That one place is in very specific scenarios where it the TypeScript compiler has already fallen down for some reason—and then leave a detailed comment explaining *why* you had to do that. (And before you do that, come ask for help in the Ember Discord! A lot of times we'll be able to get you around it another, better way!)

---

#### `unknown`

<!-- TODO: fill in the example -->

```ts
function handleInput(input: unknown): SanitizedInput {
}
```

^Now, what about the times when a function legitimately *can* handle literally *any* input you throw at it? For that, TypeScript has a different tool: `unknown`.

^Instead of `any`, most of the time you want to use `unknown`. `unknown` is like `any` in that TS doesn’t know what it is, but instead of letting you do *whatever you want* with it, TypeScript won’t let you do *anything* with it unless you explicitly do the work to figure out what type it actually is.

^ This ends up being what we actually want most of the time: we don’t know what this thing we got handed was, but we can do a little work at runtime to make handling it safe! And as we’ll talk about in a bit, TS is clever enough to understand the implications of these kinds of runtime checks!

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

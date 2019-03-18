footer: Supercharging Ember Octane with TypeScript ([bit.ly/ember-ts-2019](http://bit.ly/ember-ts-2019)) | Chris Krycho ([@chriskrycho](https://twitter.com/chriskrycho))
slidenumbers: true

[.hide-footer]

<br>

# [fit] Supercharging *__Ember Octane__*
# [fit] with *__TypeScript__*

---

## [fit] Introductions

<br>

- Chris Krycho (me)
- James Davis (TA)
- Mike North (TA)

^Hello, everyone, and welcome to the TypeScript Up Your Ember.js App workshop. I figured I’d start by introducing myself briefly and having my TAs introduce themselves.

^I’m a staff software engineer at LinkedIn, formerly at Olo. While I was at Olo, I worked on an Ember.js application which had about 60,000 lines of TypeScript. I’m also one of the maintainers of ember-cli-typescript, and an all-around nerd! We started using TypeScript in our Ember app at Olo in late 2016—*well* before it was easy or especially useful. And now at LinkedIn, I’m working on figuring out how we’re going to convert one of the largest Ember apps in the world to TypeScript, so —happily—we’re now at a point where TypeScript is both easy *and* useful!

^James Davis and Mike North are also around to help out today, and they've both been integral in making TypeScript viable in Ember over the last couple of years.

^So now I’d like to get a bit of a feel for where everyone is at in the room. We’re going to cover basically the same material no matter what, but I can pitch my discussion and adjust course and adjust speed depending on what people’s experience levels look like.

^- How many of you have written any TypeScript before?
^- What about Flow?
^- How many of you have written any typed language *at all* before?
^	- C♯ or Java?
^	- Elm, F♯, OCaml, ReasonML, PureScript, Haskell, etc.?
^- And just for giggles: Idris, F*, Agda, or Coq?

^Cool! That’s really helpful, and we’ll make a point to make sure no one gets left behind.

^I’ll say this again and again, and I really mean it: if you have a question, if something was confusing, really for any reason at all: stop me, and ask questions. I’ve intentionally left plenty of time for that and everyone in here will learn this better if you *do* ask.

---

## Introductions
### Schedule

- 1:30–2:50: *basics of TypeScript* and *TypeScript in Ember*
- 3:00–4:30: *practicing with TypeScript*

Get the code here:

- [bit.ly/ember-ts-2019](http://bit.ly/ember-ts-2019)

^Before we jump in, let’s talk through the basic approach I’m planning to take, just so everyone is on the same page.

^- From now till about 3:00, I’m going to talk through **the basics of TypeScript** and, at a very high level, **how it works in Ember** at a very high level. This is going to be kind of “lecture”-style, but *please* feel free to interrupt with questions. The point of this section is for us to go from *zero* to a point where the rest of the workshop makes good sense.

^- When we wrap that up, we’ll take a short break, till 3pm. Breaks are really important because we all have to stretch and hit the bathroom, but they’re also really important in terms of *learning*. If we just try to crunch through, all of our brains will shut down. So we’ll let ourselves chill a bit, then dive back in.

^- From 3:00 till about 4:15, we’ll work through **converting parts of the canonical “TODO MVC” Ember example app from JavaScript to TypeScript and Octane**. This will let us put into practice all the ideas we talk about in the first session. I will walk through that up here, but pausing regularly for questions, people who have gotten stuck, and so on.

^- Finally, we’ll just spend the last 15 minutes on open discussion, questions, comments, etc. – anything we can answer, we’ll be happy to.

^If any of you *have not* cloned the repository and run `yarn` to get everything set up, this first session is a good time to do that in the background. The link on the whiteboard here will take you straight to it. (https://github.com/chriskrycho/emberconf-2019)

---

<br>
<br>
<br>

## [fit] What *is* **TypeScript**?

---

## What *is* TypeScript?

- *Basically* a typed superset of JavaScript
- *Strictly* a compile-to-JavaScript language
- But close enough that we can think of it as "JS with types"

^TypeScript is *basically* a typed superset of JavaScript. I say *basically* because there are a few constructs in TypeScript which don’t exist in JavaScript. We’ll talk about those in a few minutes, but the fact that those exist means TypeScript is *strictly speaking* a distinct language which compiles to JavaScript. For most purposes, though, it’s fine to think of it as a superset of JavaScript with types.

---

## What *is* TypeScript?

- *Basically* a typed superset of JavaScript
- *Strictly* a compile-to-JavaScript language
- But close enough that we can think of it as "JS with types"

### [fit] Cool, but why should I care?

---

## What *is* TypeScript?
### Cool, but why should I care?

[.build-lists: true]

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors
3. Confident refactoring

^So that’s all well and good, but *why should you care?* Maybe that’s interesting if you’re (like me) kind of weirdly obsessed with type systems. But what does it gain you as JavaScript developer every day? How does it make your life easier?

^1. How many of you here like having docs for your functions? Now, how many of you would like it if those docs were always *right* and *up to date*? Well, the first thing about TypeScript is that that’s exactly what it gives you. My experience of using TypeScript is *not*, for the most part, the way I’ve felt in some other programming languages, where I’m writing down names of things just because. It’s more like just documenting “for this function to work *at all*, it needs you to pass in a thing that has *this property* on it”—and then finding out *in my editor* if I passed in the wrong thing, or if my function doesn’t return what the docs say it does. So that’s handy.

^2. The second thing that makes TypeScript *really great* is that it legitimately helps us ship fewer “undefined is not an object” kinds of errors to production. And I care about that not in the abstract sense but because every single one of those I ship to production means *something didn’t work for a user*. It also means it’s time I have to spend hunting down the cause of that bug instead of building something new—whether that new thing is adding a feature, or making the app work offline, or building a whole new product, or whatever else. TypeScript doesn’t get the count to zero, like some programming languages can—but it helps, a *lot*.

^3. The biggest developer experience bit is the one I've saved for last: you can refactor with *confidence*. In my experience, refactors in any significantly large JavaScript codebase are extremely difficult: it's really, really hard to know when you've actually caught *everything* impacted by a change. TypeScript lets you just start making changes and keep following the compiler's messages until you have updated *every* place affected. The first time you do that and build your project and *everything just works* feels amazing.

---

## What *is* TypeScript?
### Cool, but why should I care?

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors
3. Confident refactoring

…*and* it’s not painful to use!

^Finally, it’s worth note that it’s not *painful to use* in the way some typed languages have been. If I need to write “This function needs an object with a `quack` method on it that I can call” I can just write that inline, and we’ll see that in a few minutes! The types *cost* a lot less than they do in the sort of “typical” typed languages out there, which makes their relative value a lot higher, too.

---

## What *is* TypeScript?

<br>

### [fit] A tool to **supercharge**
### [fit] your Ember (Octane!) development

^In short, TypeScript is not just a "typed superset of JavaScript": it's a way to develop faster and more reliably, *especially* when you have to make changes! So let's dig in.

---

<br>

### [fit] *__Types__*

<br>

#### [fit] What even are they?

---

### Types
#### [fit] Types in **JavaScript**

- Primitive types: `boolean`, `string`, `number`, `symbol`
- Objects: `{ name: "Chris" }`
- Arrays: `[1, 2, 3]`
- Functions
- Classes

<br>

^There are four “primitive” types in JavaScript: strings, booleans, numbers, and symbols. There are also *objects* and *arrays*, and in their own special category, there are *classes*.

---

### Types
#### [fit] Types in **TypeScript**

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

This code has a *bug*.

```js
let me = {
  name: "Chris Krycho"
};

me.greet();
```

<br>

---

### Types
#### Compile-time and Run-time

This code has a *bug*.

[.code-highlight: 5]

```js
let me = {
  name: "Chris Krycho"
};

me.greet();
```

<br>

---

### Types
#### Compile-time and Run-time

This code has a *bug*. In JavaScript, we learn at *runtime*:

[.code-highlight: 5]

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

This code has a *bug*. In TypeScript we learn at *compile* time.

[.code-highlight: 5]

```ts
let me = {
  name: "Chris Krycho"
};

me.greet();  // COMPILE ERROR
```

> Property 'greet' does not exist on type '{ name: string; }'.

^And this is the key. As fast as your test suite might be, and as quick as you can flip over to your browser, this is *much faster* feedback. And, as we'll see, it can be more thorough than most kinds of test suites in the world for refactoring tasks. Note that that doesn't make types a *replacement* for tests, but a *wonderful complement*. You put the two together and get out something *much* better than either alone.

---

### Types

<br/>

#### [fit] Type Signatures

How do I tell TypeScript what types these things are, anyway?

^So let’s talk about how we actually write down types to use in TypeScript. That’ll give us the foundation we need for using them in the context of Ember.js specifically.

---

### Types
#### Type Declarations

type declaration = variable name + `:` + name of type

---

### Types
#### Type Declarations

type declaration = variable name + `:` + name of type

```ts
let variableName: ItsType = /* its value */;
//              ╰───────╯
//          The type signature
```

^So given that, let's talk through those kinds of types I just mentioned!

---

### Types
#### Type Declarations

Booleans:

```ts
let tsIsCool: boolean = true;
//          ╰───────╯
//     The type signature
```

---

### Types
#### Type Declarations

Numbers:

```ts
let age: number = 1;
//     ╰──────╯
// The type signature
```

---

### Types
#### Type Declarations

Strings:

```ts
let name: string = "Chris";
//      ╰──────╯
// The type signature
```

---

### Types
#### Type Declarations

Symbols:

```ts
let theAnswer: Symbol = Symbol(42);
//           ╰──────╯
//      The type signature
```

---

### Types
#### Type Declarations

Objects:

```ts
let person: { name: string } = { name: "Chris" };
//        ╰────────────────╯
//        The type signature
```

^Object types look like object literals, but with *types* instead of *values* after the name of the field.

---

### Types
#### Type Declarations

Arrays:

```ts
let languages: string[] = ["TS", "Rust"];
//           ╰────────╯
//       The type signature
```

---

### Types
#### Type Declarations

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
#### Type Declarations

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
#### Type Declarations

Arrow functions:

```ts
let length = (s: string): number => s.length;
//             ╰──────╯ ╰──────╯
//            The type signatures
```

^This works for arrow functions, too!

---

### Types
#### Type Declarations

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

[.code-highlight: 3]

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

### Types
#### Dealing with ignorance
##### `unknown`

```ts
function handleInput(maybeAge: unknown): number {
  if (typeof maybeAge === 'number') {
    return maybeAge;
  } else {
    throw new Error('not a number!');
  }
}
```

^Now, what about the times when a function legitimately *can* handle literally *any* input you throw at it? For that, TypeScript has a different tool: `unknown`. `unknown` is like `any` in that TS doesn’t know what it is, but instead of letting you do *whatever you want* with it, TypeScript won’t let you do *anything* with it unless you explicitly do the work to figure out what type it actually is.

^This ends up being what we actually want most of the time: we don’t know what this thing we got handed was, but we can do a little work at runtime to make handling it safe! And here, TS is smart enough to understand these kinds of checks. Here, TS *knows* that if you return at all, it has to be returning a `number`. It's no longer `unknown`.

^So: use `unknown` instead of `any`; it's nearly always what you want.

---

### Types

<br>

#### [fit] **Generics**

^We're in the home stretch for the *main* sets of types in TypeScript—generics!

---

### Types
#### Generics

```ts
let numbers = [1, 2, 3];       // Array<number>

let strings = ['a', 'b', 'c']; // Array<string>

let things = [   // ─╮
  { thing: 1 },  //  ├── Array<{thing: number}>
  { thing: 2 },  //  │
];               // ─╯
```

^I’m not going to spend a *lot* of time on generic types, although they’re both very *important* and very *powerful*. We’ll see some examples of them in the refactoring section, and I’ll talk about them in more detail then. However, I think it’s worth introducing them so you recognize the syntax, and talking a *little* about how to use them.

^Generics let us capture things like the fact that we can have an array of just about anything: arrays of numbers, of strings, of objects, etc. If we want to be able write that down, especially for new types *we* build, we need a syntax for it, to tell the compiler what we mean. That’s what generics are.

^You can see in the example here: an array can be an array of all sorts of things – numbers, strings, complex objects, etc. If we build up our *own* containers that can hold more than one kind of thing, we can do that with generic types. We'll see one *very* important example of this in the session where we dig into using TypeScript with Ember!

---

## Types
### Even snazzier types

- enums
- union types
- intersection types
- tuples
- literal types

^There are a handful more types you’ll see, and which can be *super* useful. I’m not going to dig particularly deep into any of these, but I did want to touch on them before we start talking about Ember.js and TypeScript together.

---

## Types
### [fit] Even snazzier: `enum` types

```ts
enum PrimaryColor {
  Red = 'FF0000',
  Green = '00FF00',
  Blue = '0000FF',
};

function fade(a: PrimaryColor, percent: number): string {/*...*/}

fade(PrimaryColor.Red, 19);
```

^A TypeScript `enum` is *basically* just a convenient way to define an object and a set of types associated with its values – to be able to say “The only thing allowed here is one of the values of this specific object.” So here, for example, we could define an RGB colors type and then we *have* to pass in one of the `PrimaryColor` keys. We can’t pass in “purple” or even another hex color code string like `8800FF`!

---

## Types
### [fit] Even snazzier: literal types

```ts
type Hallo = {
  value: 'hallo';
};

let hallo: Hallo = {
  // Type error! This isn't the *exact* string we specified!
  value: 'ahoy',
}
```

^*Literal* types are types where the only value they can have is the specific value you write down. Any kind of literal you can write in JavaScript – numbers, strings, booleans, symbols, arrays, objects, and crazy combinations of them – can be a *type* in TypeScript. So here, the `value` key in any `Hallo` type is *only* allowed to be the exact string “hallo”.

^We’ll see a handy example of how we can use this in the *next* kind of type: union types.

---

## Types
### [fit] Even snazzier: union types

```ts
type Ok = { ok: true; value: number };
type Err = { ok: false; reason: string };
type Validation = Ok | Err;

function mightFail(succeed: boolean): Validation {
  return succeed
    ? { status: 'ok', value: 42 }
    : { status: 'err', reason: '...you said to fail!' };
}
```

^Union types are literally my favorite thing in TypeScript. They let you say “this thing can be *a* or *b*.” And that’s a really common scenario! For example, we’ve all probably experienced a time when a given function needs to be able to indicate either success or failure – for example, a validation.

^With union types, we can write that out, and TypeScript will check us: if we try to return `{ ok: false, value: 12 }` or `{ ok: true, reason: "whatever, man, you're not the boss of me" }`, it will complain. Here, it’s leaning on the literal types: the `Ok` type must include *exactly* `ok: true` and `value: number` *or* `ok: false` and `reason: string`.

---

## Types
### [fit] Even snazzier: narrowing

```ts
const yay = mightFail(true); // -> Validation
if (yay.ok) {
  console.log(yay.value);
  // can't touch `yay.reason` here
} else {
  console.log(yay.reason);
  // can't touch `yay.value` here.
}
```

^What’s also neat is that once you return one of these, TypeScript can figure out the type from the `ok: true` or `ok: false`. If you’re in a place where it’s `ok: true`, you can get at `value` but not `reason`, and vice versa. As an aside: this works as a perfect complement to `unknown`!

---

## Types
### [fit] Even snazzier types: intersection types

```ts
type HasName = { name: string };
interface HasMass { mass: number }
class LivingThing { age: number }

type Being = HasName & HasMass & LivingThing;
let me: Being = { name: "Chris", mass: 72, age: 30 };
```

^An *intersection* type is the counterpart to a *union* type. Instead of saying a value can be “this *or* that” it says the value is “this *and* that.” This is kind of like doing `extends` with an `interface`… except that you can just mix and match them however you want. (And notice that you can do intersections with any kind of TS shape!)

---

## Types
### [fit] Even snazzier types: tuples

[.code-highlight: 1]

```ts
type NameAndAge = [string, number];

// valid!
let good: NameAndAge = ["Chris Krycho", 30];

// type error!
let bad1: NameAndAge = [30, "Chris Krycho"];
let bad2: NameAndAge = ["Chris Krycho", 30, { is: 'a nerd' }]
```

^TypeScript also lets use define *tuple* types. These look a little like array types, but they’re different in an important way: they have a set length, and they have a set *order*. So if we defined a type for name and age, like this…

---

## Types
### [fit] Even snazzier types: tuples

[.code-highlight: 1-4]

```ts
type NameAndAge = [string, number];

// valid!
let good: NameAndAge = ["Chris Krycho", 30];

// type error!
let bad1: NameAndAge = [30, "Chris Krycho"];
let bad2: NameAndAge = ["Chris Krycho", 30, { is: 'a nerd' }]
```

^…then this would be valid…

---

## Types
### [fit] Even snazzier types: tuples

[.code-highlight: 1, 6-8]

```ts
type NameAndAge = [string, number];

// valid!
let good: NameAndAge = ["Chris Krycho", 30];

// type error!
let bad1: NameAndAge = [30, "Chris Krycho"];
let bad2: NameAndAge = ["Chris Krycho", 30, { is: 'a nerd' }]
```

^…but these would *not*! because the order is wrong in the first one, and the second has too many values.

^These are handy for return types where you need to return more than one things – like in promise chains. If you have more complicated structures, though, you’re usually better returning objects, because names can add a lot of clarity.

---

<br>
## [fit] TypeScript—
### [fit] **Questions?**

^Okay, so that’s it for TypeScript itself. We did not cover *everything* in TypeScript, for sure, but we got through most stuff we’ll need. Any questions so far?

---

<br>

## [fit] TypeScript in Ember
<br>
### [fit] …is **mostly** just TypeScript!

^Using TypeScript in Ember is *mostly* just like using it in general, but there are a couple gotchas you should know about besides the things we'll talk through in the workshop.

---

## TypeScript in Ember
### [fit] templates === 😬

Currently, TypeScript *cannot* help us with:

- template bindings (of any sort)
- template action invocation

^*Today*, TypeScript cannot help us with template bindings of any sort, including action invocation. So we can write down the types of things passed into a component, and we can write down an action’s arguments in the component definition, but we have no guarantee that what we pass into a template matches that or that what we supply to an action is what it should be. I’ve had some early discussions with folks including core GlimmerJS developers about how we might work some magic there, and one of my own tasks at work for the next several months includes further exploration on that… but it's still a ways out.

---

## TypeScript in Ember
### [fit] sending actions === 😬

TypeScript also *cannot* help us with:

- `this.sendAction`
- `this.send`

---

## TypeScript in Ember

<br>

### [fit] `Ember.Object`
### [fit] (and everything related)

---

## TypeScript in Ember
### `Ember.Object` (and everything related)

[.code-highlight: 1]

```ts
import EmberObject from '@ember/object';

const OldSchoolPerson = EmberObject.extend({
  firstName: undefined as string | undefined,
  lastName: undefined as string | undefined,
});

class NewSchoolPerson extends EmberObject {
  firstName?: string;
  lastName?: string;
}
```

^We *can* use EmberObject with TypeScript, but there are some important caveats. Let's talk through those for a minute. First, we important it like normal.

---

## TypeScript in Ember
### `Ember.Object` (and everything related)

[.code-highlight: 1-6]

```ts
import EmberObject from '@ember/object';

const OldSchoolPerson = EmberObject.extend({
  firstName: undefined as string | undefined,
  lastName: undefined as string | undefined,
});

class NewSchoolPerson extends EmberObject {
  firstName?: string;
  lastName?: string;
}
```

^Then we *can* use it with the old `.extend` approach. But… it's kind of a mess, honestly. We have to write some *weird* type annotations if things are optional, and (though I'm not showing it here), stuff basically just falls down—especially in places like computed property definitions. This is a function of the fact that Ember's object model is *extremely* dynamic and long precedes ES6 classes.

---

## TypeScript in Ember
### `Ember.Object` (and everything related)

[.code-highlight: 1, 8-11]

```ts
import EmberObject from '@ember/object';

const OldSchoolPerson = EmberObject.extend({
  firstName: undefined as string | undefined,
  lastName: undefined as string | undefined,
});

class NewSchoolPerson extends EmberObject {
  firstName?: string;
  lastName?: string;
}
```

^So a better solution is to use the `class extends` style. This works! However, there are a couple things that can throw you for a loop here.

---

## TypeScript in Ember
### `Ember.Object` (and everything related)

- you *must* still use `init`
    - but `super.init()` not `this._super()`
- you *must* use decorators
- class properties are *not* what you might think

^Unlike all other classes, you *must* still use `init` if you want your class to get set up correctly. We did some work with the Ember Framework Core team to make `constructor` largely do the right thing (that came out in 3.6), but there were still a lot of edge cases.

^Second, you basically *must* use decorators. We'll see this in detail (and I'll explain them as we see them) in the working part of the session, but I wanted to mention it now!

^Third, class properties are *not* the same as your normal default values in the old EmberObject world. Let's see an example.

---

## TypeScript in Ember
### `Ember.Object` (and everything related)
#### [fit] Class properties are *not* what you might think

The old way…

[.code-highlight: 1-5]

```ts
import EmberObject from '@ember/object';

const TheOldWay = EmberObject.extend({
  aBadIdea: [], // FOOTGUN!
});

class TheNewWay extends EmberObject {
  aGoodIdea = []; // FINE!
}
```

---

## TypeScript in Ember
### `Ember.Object` (and everything related)
#### [fit] Class properties are *not* what you might think

The new way…

[.code-highlight: 1, 7-9]

```ts
import EmberObject from '@ember/object';

const TheOldWay = EmberObject.extend({
  aBadIdea: [], // FOOTGUN!
});

class TheNewWay extends EmberObject {
  aGoodIdea = []; // FINE!
}
```

^With a class property, though, this isn't a problem. We're not putting an item on a newly defined prototype, we're setting an *instance* property.

---

## TypeScript in Ember
### `Ember.Object` (and everything related)
#### [fit] Class properties are *not* what you might think

```ts
class Breakfast {
  waffles = 'are tasty';
}

class Breakfast {
  waffles: string;
  constructor() {
    this.waffles = 'are tasty';
  }
}
```

^In fact, these are exactly equivalent, and this is very important.

---

## TypeScript in Ember
<br>
### [fit] **Exceptions** and **Workarounds**

Some things don’t yet work like this, though.
Even in Octane. 🤕

---

## TypeScript in Ember
### Exceptions and Workarounds
#### [fit] Exception #1: Ember Data

```ts
import DS from 'ember-data';
import ComputedProperty from '@ember/object/computed';
import Metadata from 'my-app/lib/user/metadata';

export default class User extends DS.Model.extend({
	primaryName: DS.attr('string'),
	age: DS.attr('number'),
	metadata: DS.attr() as ComputedProperty<Metadata>
});
```

^The biggest problem here is Ember Data, where classes do not work *at all* without decorators. To work around this, we use the “put it all in the `.extend()` hash” trick, while still giving ourselves a named type. We *also* have to add type annotations where we don’t have a specific `DS.Transform` to point to. Here, we might know the type of a user’s “metadata” from elsewhere in the app; we just pull that type in to use with it.

^As an aside: *anywhere* we need to declare that a given item will be a computed property, this is how to do it. And in our app we actually usually use `CP` as the import name, but I wrote it out explicitly here to be clear.

---

## TypeScript in Ember
### Exceptions and Workarounds
#### [fit] Exception #2: Ember CLI Mirage

```ts
import Mirage, { faker } from 'ember-cli-mirage';

export default class User extends Mirage.Factory.extend({
	firstName: faker.name.firstName(),
	lastName: faker.name.lastName(),
}) {}
```

^Most of the same considerations apply with Mirage, and apparently for the same kinds underlying reasons. We use the same trick to get around it: add a class that `extends` from a normal `Mirage.Factory.extend()` invocation. That way we can use the actual class name and type and shape where we need it. I and James have both independently worked on getting good types for Mirage in place in apps we've worked on, and I hope we can land those in open-source—perhaps as part of Mirage's upcoming 1.0 push.

---

## TypeScript in Ember
### Exceptions and Workarounds
#### [fit] Workaround: "Type Registries"

^This brings us to the last thing we need to talk about, a weird thing you will see when using the TypeScript generators: a *type registry*.

---

## TypeScript in Ember
### Exceptions and Workarounds
#### Workaround: "Type Registries"

```ts
import { inject as service } from '@ember/service';
import Component from '@glimmer/component';
import DS from 'ember-data';
import User from 'my-app/models/user';

export default class UserProfile extends Component<User> {
  @service store!: DS.Store;

  @action updateEmail(email: string) {
    const user: User = this.store.peekRecord('user', this.args.id);
    //        ╰this╯
  }
}
```

^We want to be able to avoid massive boilerplate that TS *should* just understand everywhere.

---

## TypeScript in Ember
### Exceptions and Workarounds
#### Workaround: "Type Registries"

[.code-highlight: 10-11]

```ts
import { inject as service } from '@ember/service';
import Component from '@glimmer/component';
import DS from 'ember-data';
import User from 'my-app/models/user';

export default class UserProfile extends Component<User> {
  @service store!: DS.Store;

  @action updateEmail(email: string) {
    const user: User = this.store.peekRecord('user', this.args.id);
    //    ╰────────╯
  }
}
```

^Here, we have to write a type annotation so that we know what kind of thing we're dealing with. This is both error prone – we could write the type annotation wrong! – and annoyingly repetitive.

---

## TypeScript in Ember
### Exceptions and Workarounds
#### Workaround: "Type Registries"

[.code-highlight: 10-11]

```ts
import { inject as service } from '@ember/service';
import Component from '@glimmer/component';
import DS from 'ember-data';
import User from 'my-app/models/user';

export default class UserProfile extends Component<User> {
  @service store!: DS.Store;

  @action updateEmail(email: string) {
    const user = this.store.peekRecord('user', this.args.id);   //
    //    ╰──╯
  }
}
```

^If we use a type registry, we can make it so we don't need that. Typing `peekRecord('user', ...)` just does the right thing: TypeScript understands at compile time what Ember understands at run time. It's less error prone and it's less annoying!

---

## TypeScript in Ember
### Exceptions and Workarounds
#### Workaround: "Type Registries"
#### An example Ember Data registry entry

```ts
import DS from 'ember-data';

export default class User extends DS.Model {
  @attr('string') email!: string;
}

// DO NOT DELETE: this is how TypeScript knows how to look up your models.
declare module 'ember-data/types/registries/model' {
  export default interface ModelRegistry {
    'user': User;
  }
}
```

^With just this, we’ve integrated our model into the registry! Most apps will end up having a *bunch* of files with these: basically, all your model definitions. This leans hard on a couple tricks the TypeScript compiler has up its sleeve: it will automatically merge *modules* and *interfaces* if you declare them in separate places. By having this registry, we can tell TypeScript in the official Ember Typings that when you write `store.find('user')` or similar, what it returns is the type that this registry says the `"user"` key gets – so, here, the `User` model! And we pull the exact same trick with services, controllers, and Ember Data, adapters, serializers, etc.

---

<br>

## [fit] TypeScript in Ember

Mostly just normal TypeScript!

…except for EmberObject and Ember Data and Mirage!

---

<br>

## [fit] TypeScript in Ember
### [fit] **Questions?**

---

## [fit] **Break!**

<br>

### [fit] Water 🚰
### [fit] stretch 🙆‍♀️ 🙆‍♂️
### [fit] bathroom 🚽…

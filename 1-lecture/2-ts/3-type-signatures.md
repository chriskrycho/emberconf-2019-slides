### Type signatures

How do we actually write down these types?

Note: So let’s talk about how we actually write down types to use in TypeScript. That’ll give us the foundation we need for using them in the context of Ember.js specifically.

---- 

#### Basic types

- Primitive types: `boolean`, `string`, `number`, `symbol`
- Objects: `{ name: string }`
- Arrays: `Array<number>` or `number[]` 

Note: There are four “primitive” types in TypeScript: strings, booleans, numbers, and symbols. There are also *object* and *array* types.

- Object types look like object literals, but with *types* instead of *values* after the name of the field.
- Array types can be written two ways: as `Array<{the type, like "number" here}>` or `{the type, like "number" here}` followed by `[]`. We’ll come back to the version with `Array` written out explicitly in a few.

---- 

#### `any`

`any`: the great (and *terrible*) escape hatch.

```ts
function madWithPower(noLimits: any) {
  return noLimits.noHelpEither.ohNo;
}
	
	madWithPower("just a string");  // 💥 at runtime
// "TypeError: undefined is not an object
//     (evaluating 'noLimits.noHelpEither.ohNo')"
```

Note: TypeScript gives us an escape hatch, and I’ll tell you about it up front because it’s a useful tool while you’re converting your codebase, and for *rare* occasions after that. But it’s also *dangerous*.

The `any` type is exactly what it sounds like. You’re telling TypeScript, “This can be anything, and don’t check me on *anything* I do with it.” Worse, it *infects* its context. Once you use it for a given item, TS basically throws away everything it knows about that item.

That means TypeScript *cannot help you* with anything marked as being of the type `any`. No autocompletion. No type errors. Nothing. `any` is an escape hatch, but once you *do* get your types written down, you should use it as a tool of last resort and be *very* careful with runtime checks when you do bust it out.

---

#### `any`

```ts
// As of TypeScript 3.4, the compiler doesn’t understand that
// `somethingFine` is equivalent with the type `Neato`. See
// <https://github.com/Microsoft/TypeScript/issues/12345> for
// details.
let somethingWeird = (somethingFine as any) as Neato;
```

Note: I strongly recommend that you *never* use `any` except in very specific scenarios where it the TypeScript compiler has already fallen down for some reason—and then leave a detailed comment explaining *why* you had to do that.

Now, what about the times when a function legitimately *can* handle literally *any* input you throw at it? For that, TypeScript has a different tool: `unknown`.

---

#### `unknown`

<!-- TODO: fill in the example -->

```ts
function handleInput(input: unknown): SanitizedInput {
}
```

Note: instead of `any`, most of the time you want to use `unknown`. `unknown` is like `any` in that TS doesn’t know what it is, but instead of letting you do *whatever you want* with it, TypeScript won’t let you do *anything* with it unless you explicitly do the work to figure out what type it actually is.

This ends up being what we actually want most of the time: we don’t know what this thing we got handed was, but we can do a little work at runtime to make handling it safe! And as we’ll talk about in a bit, TS is clever enough to understand the implications of these kinds of runtime checks!

---

#### Type ascriptions

<!-- TODO: move this -- it’s very late in the lecture -->

Let’s write some basic types!

	let myName: string = "Chris";
	let theAnswer: number = 42;
	let aLie: boolean = false;
	
	function stringLength(s: string): number {
	  return s.length;
	}
	
	let toString = (n: any): string => `${n}`;

Note: So when you’re using these types in your program, you’ll need to write down some types! Here are some types of things you might want to know how to write down:

- the primitive types
- functions

---- 

##### Type inference

Most of those we didn’t actually need!

	let myName = "Chris";  // automatically `string`
	let theAnswer = 42;  // automatically `number`
	let aLie = false;  // automatically `boolean`
	
	function stringLength(s: string) {
	  return s.length;
	}  // automatically returns `number`
	
	// automatically returns string
	const toString = (n: any) => `${n}`;

Note: A lot of times, you *won’t* have to write down types. Anywhere you assign a value, TypeScript *infers* the type for you. Similarly, TypeScript will figure out function return types for you.

---- 

###### Type inference: quite sophisticated

	// returns `string | { say: string }`
	function moreComplicated(a: boolean) {
	  return a ? 'yay' : { say: 'what' };
	}
	
	// Type error! We haven't checked whether `say` exists, and
	// in fact it *doesn't*, so this would blow up. But TS will
	// save us.
	console.log(moreComplicated(true).say);

Note: TypeScript can infer a *lot*. It’ll even infer more interesting types we haven’t talked about yet, like *union* types. Here, TypeScript knows that we’re returning *either* a string or an object with a key named “say” which has a string value. And when we go to use it, we’ll have to check what the type is, or TS will yell at us.

---- 

###### Type inference: limits

	let aBunchOfThings = []; // has type `any[]`
	
	function whatIsThis(untypedThing) {
	  // Type error! We don't know that `untypedThing` *has*
	  // a `length` property! It's actually `any` here.
	  return untypedThing.length;
	}

Note: But it can’t infer *everything*. For example, if you create an empty array, you’ll need to tell it what *kind* of array, or it’ll fall back to the `any` type.

You also have to write function *argument* types explicitly pretty much all the time; TS doesn’t try to do total program inference like some other languages do.

Finally, when using *generic types*, which we’ll talk about in a minute, it will eventually fall over even when *you* can see that there’s only a single type it could be. In that case, you *do* have to write things out explicitly.

---- 

#### Writing shapes

Since TS is all about shapes, let’s write some!

- `type` for “type aliases”
- `interface` for “extensible shapes”
- `class` for JS classes with annotations

Note: Since TypeScript is all about shapes, how do we write them

---- 

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

---- 

##### Writing shapes: `interface`

	interface Name {
	  primary: string;
	}
	
	interface WesternName extends Name {
	  middle?: string;
	  last: string;
	}


Note: Another way to write a shape is with an `interface`, which can be *extended* and *implemented*. These are basically interchangeable with type aliases, except for those two differences. Here, the `WesternName` has all the properties of `Name` and adds in an optional `middle` and required `last` name. You can use these wherever you’d use a type alias… but also with classes!

---- 

##### Writing shapes: `class`

TypeScript classes are just JavaScript classes with type annotations.

Note: A TypeScript class is *basically* just a JavaScript class with type annotations for all the bits attached to it.

---- 

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

---- 

##### Writing shapes: `extends` and `implements`

	class Person implements Name {
	  first: string;
	  age: number;
	}
	
	class Programmer extends Person {
	  knownJSFrameworks: string[];
	}

Note: classes can also implement interfaces and extend other classes. If they declare they implement an interface, TypeScript will check that everything the interface has, the class has! Meanwhile `extends` works just like it does in normal JavaScript: you attach to the prototype of the type you’re extending.

---- 

##### Writing shapes: when to use each

- `type` as the default
- `interface` for defining shapes for more than one `class` to conform to
- `class` for a convenient way to get a shape and a constructor at the same time

Note: My basic tack is I start with a `type` alias, and rarely go beyond that. That’s where you really get the “this is just documentation my editor helps me check!” approach. I switch to an `interface` only if I’m going to define multiple `class`es that need to implement a certain shape contract. And I use `class` pretty rarely *other* than when I’m building Ember components or services or whatever. (My own code is mostly just functions, and `type` aliases work *great* with functions!)

I should note: I’m offering an opinionated take here. This actually runs up against Microsoft’s view a little bit – they basically say to use interfaces for everything and not to use type aliases at all, because things should *always* be open to being extended. I disagree! I often want to just write down a bunch of small types like LEGO blocks to fit together. But there’s room for different styles here, in any case.

---- 

#### “nullable” types: getting a handle on `null` and `undefined`!

- the “optional” annotation
- strict null checking

Note: Next, one of the most important things TypeScript can help us with—I would argue, at least!—is `undefined` and `null`. How many people in this room have seen “undefined is not an object” or similar in the last week?  Same. We’re slowly eradicating them from our app, but… it’s taking a while.

TypeScript gives us two tools we can combine to help us fix this problem: *optional* type annotations, and the *strict null checking* compiler option.

---- 

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

---- 

##### “nullable” types: strict null checking

Turn on `"strictNullChecking": true` in `tsconfig.json`!

	let el: HTMLElement = document.querySelector('some-id');
	el.focus(); // Type error! This could be `null`!

Note: We can combine optional declarations with the `"strictNullChecking"` compiler option in our `tsconfig.json` file, which is where *all* the compiler options go. We’ll look at that file a bit in the second session. For now, it’s just important to know that if we turn on `strictNullChecking`, anywhere that *could* be `null` or `undefined`, TypeScript will require us to check for it! This can be a little annoying, but it means fewer bugs in production.
  
If you’re starting a *new* Ember app with TypeScript, I’d turn this flag on at the start. If you’re dealing with an existing app... well, that’s probably going to be too hard, but it’s worth aiming to get there eventually!

---- 

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

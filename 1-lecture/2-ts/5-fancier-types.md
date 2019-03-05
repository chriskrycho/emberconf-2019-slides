### Even snazzier kinds of types

- enums
- union types
- intersection types
- tuples
- literal types

Note: There are a handful more types you’ll see, and which can be *super* useful. I’m not going to dig particularly deep into any of these, but I did want to touch on them before we start talking about Ember.js and TypeScript together.

---- 

#### `enum`

	enum PrimaryColor {
	  Red = 'FF0000',
	  Green = '00FF00',
	  Blue = '0000FF',
	};
	
	function fade(a: PrimaryColor, percent: number): string {
	  // ...
	}
	
	convert(PrimaryColor.Red, 19);

Note: A TypeScript `enum` is *basically* just a convenient way to define an object and a set of types associated with its values – to be able to say “The only thing allowed here is one of the values of this specific object.” So here, for example, we could define an RGB colors type and then we *have* to pass in one of the `PrimaryColor` keys. We can’t pass in “purple” or even another hex color code string like `8800FF`!

---- 

#### literal types

	type Hallo = {
	  value: 'hallo';
	};
	
	let hallo: Hallo = {
	  // Type error! This isn't the *exact* string we specified!
	  value: 'ahoy',
	}

Note: *literal* types are types where the only value they can have is the specific value you write down. Any kind of literal you can write in JavaScript – numbers, strings, booleans, symbols, arrays, objects, and crazy combinations of them – can be a *type* in TypeScript. So here, the `value` key in any `Hallo` type is *only* allowed to be the exact string “hallo”.

We’ll see a handy example of how we can use this in the *next* kind of type: union types.

---- 

#### union types

	type Ok = { ok: true; value: number };
	type Err = { ok: false; reason: string };
	type Validation = Ok | Err;
	
	function mightFail(succeed: boolean): Validation {
	  return succeed
	    ? { status: 'ok', value: 42 }
	    : { status: 'err', reason: '...you said to fail!' };
	}

Note: Union types are literally my favorite thing in TypeScript. They let you say “this thing can be *a* or *b*.” And that’s a really common scenario! For example, we’ve all probably experienced a time when a given function needs to be able to indicate either success or failure – for example, a validation.

With union types, we can write that out, and TypeScript will check us: if we try to return `{ ok: false, value: 12 }` or `{ ok: true, reason: "whatever, man, you're not the boss of me" }`, it will complain. Here, it’s leaning on the literal types: the `Ok` type must include *exactly* `ok: true` and `value: number` *or* `ok: false` and `reason: string`.

---- 

#### union types

	const yay = mightFail(true);
	if (yay.ok) {
	  console.log(yay.value);
	  // can't touch `yay.reason` here
	} else {
	  console.log(yay.reason);
	  // can't touch `yay.value` here.
	}

Note: What’s also neat is that once you return one of these, TypeScript can figure out the type from the `ok: true` or `ok: false`. If you’re in a place where it’s `ok: true`, you can get at `value` but not `reason`, and vice versa.

---- 

#### intersection types

	type HasName = { name: string };
	interface HasMass { mass: number }
	class LivingThing { age: number }
	
	type Being = HasName & HasMass & LivingThing;
	let me: Being = { name: "Chris", mass: 72, age: 30 };

Note: An *intersection* type is the counterpart to a *union* type. Instead of saying a value can be “this *or* that” it says the value is “this *and* that.” This is kind of like doing `extends` with an `interface`… except that you can just mix and match them however you want. (And notice that you can do intersections with any kind of TS shape!)

---- 

#### tuples

	type NameAndAge = [string, number];
	
	
	
	
	
	
	

Note: TypeScript also lets use define *tuple* types. These look a little like array types, but they’re different in an important way: they have a set length, and they have a set *order*. So if we defined a type for name and age, like this…

---- 

#### tuples

	type NameAndAge = [string, number];
	
	// valid!
	let good: NameAndAge = ["Chris Krycho", 30];
	
	
	
	

Note: then this would be valid…

---- 

#### tuples

	type NameAndAge = [string, number];
	
	// valid!
	let good: NameAndAge = ["Chris Krycho", 30];
	
	// type error!
	let bad1: NameAndAge = [30, "Chris Krycho"];
	let bad2: NameAndAge = ["Chris Krycho", 30, { is: 'a nerd' }]

Note: … but these would *not*! because the order is wrong in the first one, and the second has too many values.

These are handy for return types where you need to return more than one things – like in promise chains. If you have more complicated structures, though, you’re usually better returning objects, because names can add a lot of clarity.

----


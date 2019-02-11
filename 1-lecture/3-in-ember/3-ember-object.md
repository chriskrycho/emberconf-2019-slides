### `Ember.Object` (and everything related)

	import EmberObject from '@ember/object';
	
	const OldSchoolPerson = EmberObject.extend({
	  firstName: undefined as string | undefined,
	  lastName: undefined as string | undefined,
	});
	
	class NewSchoolPerson extends EmberObject {
	  firstName?: string;
	  lastName?: string;
	}

Note: The first thing we have to deal with is `Ember.Object`. Mostly, things here work *fine* using either the classic `.extend()` method or new-style classes. This goes direct uses of `Ember.Object`, like in this example, but it also goes for components, services, controllers, routes… *almost* everything in the Ember world. Note that *almost* everything I’m about to say here applies equally to classes in JavaScript.

---- 

#### `class` properties

	import Component from '@ember/component';
	import { computed } from '@ember/object';
	import { assert } from '@ember/debug';
	
	export default class UserProfile extends Component {
	  name!: string;
	  age!: number;
	
	  desc = computed('name', 'age', function(this: UserProfile): string {
	    return `${this.name} is ${this.age} years old!`;
	  });
	
	  hobbies: string[] = [];
	
	  constructor() {
	    super();
	    assert('`name` is required', !!this.name);
	    assert('`age` is required', !!this.age);
	  }
	}

Note: the syntax is a little different. Let’s walk through this *small* example just to give you a taste. (We’ll cover this all again in the next session when we’re actually using TypeScript, so don’t worry if it doesn’t *stick* just yet.)

- We declare the types of the properties that get passed in, `firstName` and `lastName`. They get an exclamation point here to tell TypeScript “this will *definitely* be initialized” even though we don’t set it up in the constructor.
- Doing it *this* way, we *assign* properties with `=` instead of doing it key-value style with `:`
- We can assign an array as a property directly; we don’t have to put it in `init` to avoid sharing between instances – because these *are* instance properties.
- In the `computed` property callback, we specify the `this` type. More on that in just a minute!
- We use the normal JavaScript class `constructor` instead of the Ember-specific `init` method. You *can* use `init` but don’t need to for normal setup stuff anymore.

You may also have noticed that I didn’t do `.get()` here. That’s because I’m doing *everything* from this point in the workshop forward with Ember 3.1 in mind. For nearly everything, we just get to do `this.name` instead of `this.get('name')`. (Proxied values are the only exception!)

---- 

#### So which should I use?

Mostly, just use `class`-style declarations!

They’re better, *and* easier to type-check!

Note: For most basic things, you can and should just use the class style of declaration everywhere! It’s nicer, and it’s *much* easier to get type-checking, in part because of some of the hoops we have to jump through to get TypeScript playing nicely with all the neat-but-slightly crazy things Ember Object can do.

---- 

#### Things `class` can’t do

	import Component from '@ember/component';
	
	export default class UserList extends Component {
	  tagName = 'ul'; // NOPE 👎
	  items!: User[];
	}

Workarounds:

- combine `.extend()` with `class`
- use decorators!

Note: There *are* a few things `class` declarations cannot do that we rely on in Ember. The most important of these is setting or merging definitions on the prototype, rather than on a per-instance basis. That’s a pretty technical description; let’s make it concrete. When we create a `Component`, we can choose what its containing HTML tag should be with the `tagName` property. If we try to do this with a `class` property declaration, it simply won’t work. There are a couple workarounds for this.

##### Prototype workarounds: `extend` with `class`

	import Component from '@ember/component';
	
	export default class UserList extends Component.extend({
	  tagName: 'ul',
	}) {
	  items!: User[];
	}

Note: this is weird, but it works! We’ll see it again in a bit, too.

##### Prototype workarounds: decorators

	import Component from '@ember/component';
	
	@tagName('ul')  // 👍 works!
	export default class UserList extends Component {
	  items!: User[];
	}

Note: The other (better) option is decorators – which work for *most* of these oddities. We’ll see a *bunch* of these in way more detail in the second session as we walk through converting part of an app to TypeScript!

---- 

##### Using `class` and `.extend()`

	class NewSchoolPerson extends EmberObject {
	  firstName?: string;
	  lastName?: string;
	}
	
	// NOPE 👎
	const WithAMiddleName = NewSchoolPerson.extend({
	  middleName: '',
	});


Note: However, we should pretty much *always* use classes with one exception: you cannot do `.extends()` and pass in a hash on a class you’ve defined with `class`. (This is all true whether you’re in TypeScript or not, but switching to TypeScript is the first place a lot of people run into it.) That means if you have a place where you *need* that kind of prototype-merging we talked about a minute ago, *and* you have a deep inheritance hierarchy, all the places back up the inheritance chain need to be defined as old-school Ember Objects, *not* as classes.

This is a very practical reason to follow what is good advice anyway: avoid deep inheritance chains!

---- 

#### `this` types: example

	import Component from '@ember/component';
	import { computed } from '@ember/object';
	import { assert } from '@ember/debug';
	
	export default class UserProfile extends Component {
	  name!: string;
	  age!: number;
	
	  desc = computed('name', 'age', function(this: UserProfile): string {
	    return `${this.name} is ${this.age} years old!`;
	  });
	
	  // snip
	}

Note: As I commented above, we sometimes have to write down the `this` type for TypeScript to know what’s going on. You *normally* won’t have to worry about this, because some of the changes for Ember 3.1 help, and so do decorators. The biggest times we have to use this are with old-style computed properties and old-style `actions` hashes. If we didn’t have `this: UserProfile` here, this wouldn’t be able to check that we’re setting a legitimate value, because the `this` context for actions (which Ember sets for us behind the scenes) wouldn’t be known to TypeScript. In both cases, decorators give us a nicer solution, but you *will* see this if you’re converting existing code, so it’s worth knowing about.

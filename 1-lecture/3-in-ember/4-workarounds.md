### Exceptions and Workarounds

Some things don’t work like this, though. 🤕

---- 

#### Ember Data

	import DS from 'ember-data';
	import ComputedProperty from '@ember/object/computed';
	import Metadata from 'my-app/lib/user/metadata';
	
	export default class User extends DS.Model.extend({
	  primaryName: DS.attr('string'),
	  age: DS.attr('number'),
	  metadata: DS.attr() as ComputedProperty<Metadata>
	});

Note: The biggest problem here is Ember Data, where classes do not work *at all* without decorators. To work around this, we use the “put it all in the `.extend()` hash” trick, while still giving ourselves a named type. We *also* have to add type annotations where we don’t have a specific `DS.Transform` to point to. Here, we might know the type of a user’s “metadata” from elsewhere in the app; we just pull that type in to use with it.

As an aside: *anywhere* we need to declare that a given item will be a computed property, this is how to do it. And in our app we actually usually use `CP` as the import name, but I wrote it out explicitly here to be clear.

---- 

#### Ember CLI Mirage

Also doesn’t play nicely with classes. 😭

Get around it the same way:

	import Mirage, { faker } from 'ember-cli-mirage';
	
	export default class User extends Mirage.Factory.extend({
	  firstName: faker.name.firstName(),
	  lastName: faker.name.lastName(),
	}) {}

Note: Most of the same considerations apply with Mirage, and apparently for the same kinds underlying reasons. We use the same trick to get around it: add a class that `extends` from a normal `Mirage.Factory.extend()` invocation. That way we can use the actual class name and type and shape where we need it, but don’t have to 

---- 

##### Ember CLI Mirage – Registries

To make Ember CLI Mirage ergonomic in TypeScript, we need a type registry!

Secret: I have built one in our app and will be upstreaming it soon!

(Double secret: It’ll come with a quest issue to *type the whole ecosystem*.)

----
# Session 2: Converting an Ember App



## “Install”

- `git checkout start` – this is the TODO MVC app with the latest Ember 3.1 beta, with no changes made to it! This is just a plain-old Ember.js app.

- Next, we’ll “install” the addons we need! However, you’ll only need to *actually* install the addons if you didn’t download the zip file I sent, but cloned the repository instead. Either way: do `git checkout install`. Then, if you didn’t download the zip, you’ll need to run `yarn`. And pray the internet holds up while you do. 😬

	Here’s everything we’re “installing”:

	- `ember-cli-typescript` and the pieces it knows we’ll need:
		- `typescript` (v. 2.8)
		- `@types/ember`
		- `@types/ember-data`
		- `@types/ember-test-helpers`
		- `@types/ember-testing-helpers`
		- `@types/rvsp`
	- `@ember-decorators` with sub-packages that line up with the Ember modules API. So e.g. `import { computed } from @ember/object`; → `import { computed } from @ember-decorators/object;`.
		- We’re also setting `"experimentalDecorators": true` in our `tsconfig.json` file.

Now, let’s go ahead and spin up the app by running `ember serve` so we can see that what we’re doing *works* all along the way.

## Convert

### `todo-list.js`

- Rename `todo-list.js` to `todo-list.ts`.
- Celebrate! 🤩 You now have TypeScript in your application!
- But we don’t have any *meaningful* or *useful* type-checking yet. Try changing `this.set("canToggle", true)` to `this.set("canToggle", "waffles")`, for example. That shouldn’t work! TypeScript is supposed to save us from this kind of thing!
- Convert this to a `class` instance by adding `class TodoList extends` before `Component.extend` and replacing the `;` at the end of the component definition with an empty `{}`.
- Now use the fact that we have a named class to tell TypeScript about the type of `this` in the actions methods.

And now we have type errors! So let’s fix them.

### Start fixing type errors

There are two errors we see here:

1. `this.todos` – because we haven’t told TypeScript that `todos` exist on the class at all.
2. `this.repo.persist` – because it doesn’t know what exactly `this.repo` is. In fact, it think it’s an `Ember.ComputedProperty<Service>`, because the Ember typings have to support the pre-3.1 typings.

We’ll fix the problem with `todos` iteratively, getting to *type-checking*, then to *type-checking helpfully*.

- Add a property declaration to the class body. We had `{}`, now we want to add `todos: any[]` in there to indicate we have a an array, but it’s of `any` – we just want TS to basically not check anything other than that it’s an array (because that’s kind of all we know right now).

	This is often a good *starting point* strategy for a complicated file when you’re just getting going.

- This still doesn’t type-check in `toggleAll()` in the `actions` hash, though. Why? Because the `this` type for a method in the actions hash is… the actions hash, as far as TypeScript is concerned. But Ember.js automatically binds those to the surrounding context. We need a way to tell TypeScript about that. We can use a `this` type annotation. TypeScript basically treats every method like it has an invisible `this` parameter, and it also lets us make it visible and specify the type. So we can write `this: TodoList` here and that will work!

- Now, we can poke around the codebase and see if we can figure out what the actual type of a ”todo” item is. I’ll give you a few to poke around and try to find it. \<pause\> So this is one of the things I find *super* helpful about TypeScript: for things like this it ends up being documentation that stays up to date!

- For right now, we’ll just write it in this file. `type Todo = { title: string; completed: boolean }`. Later, we can put it somewhere more useful.

Now, what about the `repo` problem? We have a good solution for this in the old world of Ember, and I can show you that on the next break if you want, but at this point we have decorators, so we’re better off just using those!

- Remove `import { service } from '@ember/service';`. Add `import { service } from '@ember-decorators/service';` to replace it.

- Remove the `repo: service()` line from the body of the `.extends()` hash.

- In the class body, add `@service repo;`. Note that the type error went away, but this is really only part of the solution. We could type `this.repo.goOutForLunch` and it would still type-check.

- Let’s get TS to help us with this! Open your `tsconfig.json` file, and in the `"compilerOptions"` hash, add `"noImplicitAny": true` – the best spot is probably right below `"noImplicitThis": true`. You’ll probably need to reload the project in your editor for this to get picked up.

- \<once everyone has gotten there\> Now we get an error on the line `@service repo`, because it implicitly has the type `any`, and we just told TS to stop letting this pass! Now we need to tell TS what the type is. We *could* just write `any`, and that would work \<demo on screen\> – but we can fix this pretty easily, so let’s do that!

- Open `app/services/repo.js`. Change it to a class declaration in *exactly* the same way we did with the `TodoList` class, leaving everything in the body of the `.extends()` call.

- We could quiet the type errors in *here* by adding `: any` the definitions of `attrs` in `add()` and `todo` in `delete()`. But, we already wrote down the type of a to-do item, so we can just reuse that! Let’s go ahead and put it in its own file.

	- You can create something like `app/todo.ts` for this.
	- Then move the `type Todo = ...` declaration to that file and write `export` in front of it. We can also make it the default export from that module.
	- Now, in both `todo-list.ts` and `repo.ts`, we can `import Todo from '../todo`.

- Back in `repo.ts`, we can make `add` and `delete` just take a `Todo` as their argument. And now the `repo` service fully type-checks!

- Now, let’s go back to `todo-list.ts`. Now that we have a class for the service, we can do `import Repo from '../services/repo';`. Then we can add that to the service declaration in the class: `@service repo: Repo`.

	- If we have our editor configured for TypeScript, it can probably do this *for* us. I don’t know for sure how Emacs or Vim will do with this, but I know that the TypeScript support in VS Code, Atom, and JetBrains IDEs all support automatically importing things when you autocomplete.

	- Now we can confirm that it’s being imported and checked correctly: if we type `this.repo.add()` but don’t give it anything, we’ll see a type error!

One of the things this shows you is that you have a tradeoff as you’re converting a file. At any point along the way in here, we could have just paused and said, “Nope, I don’t want to go any further down this particular rabbit hole” and used `any` as our escape hatch. That’s a *really* important thing to remember when you’re working through your codebase. Unless you have a *really* small app, or a long block of time you can dedicate to it (and in my experience that’s pretty rare)… you’re going to *have* to do it piecemeal. So you just get as far as makes sense and then `any` your way out and just come back and slowly remove your `any`s.

CHECKPOINT

### More type fixing

Okay, so we’ve made a bunch of progress, but there are a coupule things that still aren’t great here.

1. We still have most of the definition in an old-fashioned `.extend()` instead of in the body of the class definition.
2. We have a few things that type-check… but aren’t really safe.

And those two things are connected. If you look at the type of `this` in the `allCompleted` computed property (hovering over it, or triggering whatever key combination shows types locally), etc. you’ll see that the `this` type is `any` – and IntelliJ IDEs are smart enough to just flag this up for you automatically.

- Let’s try to make the `this` there correct. Add the same kind of type annotation we did in the actions. \<pause, let everyone see it’s broken\> Now we see an error on the whole class, and also the reference to `this.allCompleted` doesn’t type-check anymore. The error on the whole class is `'TodoList' is referenced directly or indirectly in its own base expression.` There are certain times where referring to a class we’ve named in a `.extend()` expression triggers this, and this is one of them. We need the computed property to live on the class definition instead.

- We can *start* by just assigning the property inline on the class as a class property: `allCompleted = computed('todos.@each.completed', ...)`. \<demo this\>

	- However, this has a really important side effect that probably isn’t obvious just looking at it.
	- Things in `.extend` blocks get defined *once*, on the prototype.
	- Things defined with this kind of assignment in a class body are the same as defining something in the `constructor` – so you get a *new copy* with every single instance.
	- That can be *handy* for times when you *want* a new copy of something between every single instance. But in cases like this, we don’t.
	- We only need once copy of these functions – we’d take a pretty big performance hit if we had a few thousand of these and therefore a few thousand extra copies of this function floating around!

- Solution: let’s use ES5 getters and decorators! Since we’re living on the edge and using the Ember 3.1 beta, and since Chris Garrett has rebuild the `ember-decorators` so they work, we can use the `@computed` decorator.

	- Remove `computed` from the import from `@ember/object`.
	- Add `import { computed } from '@ember-decorators/object`.
	- Down below, we’re going to rewrite this using the decorator.
		- First, write `@computed()` in the class body. \<show it\>
		- Now, copy the string key from the existing declaration and paste it in as the argument of that decorator. \<show it\>
		- Now, take the function declaration from the existing definition and paste it immediately below the computed decorator.
		- Replace `function` with `get allCompleted`
		- Delete the original computed declaration!
	- Aaaand, that’s it! Now everything is type-checking: the type of `this` (and, just as importantly, `this.todos`) in the body of the computed is correct, and anywhere we do `this.allCompleted`, that has the type `boolean` – check out the type in `toggleAll`, for example.

CHECKPOINT

### Cleanup

Okay, so we have everything type-checking the way it should be now. However, we have a bunch of stuff that’s still stuck in the `.extend()` block, and it would definitely be nice to get it all in the class, and to get rid of some of that noise along the way! So now we’re going to push everything else forward using decorators!

#### `@action`s

 - Let’s start by pulling in the `@action` decorator to replace the actions hash: add `action` to the imports from `@ember-decorators/object`.
- Now, for each method in the `actions` hash:
	- copy it to the body of the class as a top-level method.
	- right above each one, add the `@action` decorator
- remove the actions hash entirely
- Now, check on your app again. Everything should still be working!

#### Everything else

We only have three fields left to deal with!

- The first ones we’ll tackle are the simplest: `elementId` and `canToggle`. These can just become normal class properties. We pull them out of the `.extend()` hash and put them as *assignments* in the body of the class: `elementId = 'main';` and `canToggle = true`. That’s it!

- The next simplest is `tagName `: we can set that on the class using a *class decorator*.

	- Add `import { tagName } from '@ember/component';` to the imports.
	- Add `@tagName('section')` immediately before the `export default class TodoList`.
	- Delete the `tagName: 'section'` from the body of the `.extend()` hash.

At this point, there’s nothing left in the hash *at all*, so we can just delete it in favor of having the plain old `class` declaration.

We’re *almost* done, but not quite: we can go ahead and get rid of these last few `this: TodoList` declarations. Delete them, and you’ll notice that you get errors anywhere you have `this.set()`. However, and this is admittedly strange, but we can use the generic `set` function. And we already have it imported! So replace `this.set(...)` with `set(this, ...)` and everything works!

Now, there are two things to note that we didn’t cover here:

- properties which have *default* values but which can be changed by whatever invokes the component (in the Handlebars template)
- properties which must be *merged* with things up the prototype chain

Unfortunately, nothing in the TODO MVC app gives us a good example of either of these problems. We can imagine them here, though.

#### Default values

For the default value issue, let’s act as if `canToggle` were something that could change depending on something outside this particular component – for example if opted to show a read-only view if we went offline.

It might *appear*, if you’re not already really familiar with the semantics of class property assignments vs. the semantics of `.extend()` initialization (and let’s be honest: most people do *not* make a habit of getting intimately familiar with that just for kicks), that this is the same as it was before, just using class syntax. But it’s not. This is just syntax sugar for writing an assignment in the constructor.

The upside to that is that if you’ve ever had problems from where you did `.extend({ someArray: [] })` and found the `someArray` shared between instances… they just go away with a class property initialization. But the downside is that it simply *overrides* any value you passed in: values passed in get set *before* the constructor runs. So we need to check whether the value is already set. You can do that inline using `isNone` from `@ember/utils`. That ends up looking like `canToggle: boolean = isNone(this.canToggle) ? true : this.canToggle;`. Most utility libraries out there have a convenience function for this kind of thing.

At Olo we’re using `_.defaultTo`, which just ends up looking like `canToggle: boolean = defaultTo(this.canToggle, true);`.

Once the decorator spec settles further, we’ll actually just be able to use an `argument` decorator and do `@argument(true) canToggle: boolean` here.

#### Merged properties

The other problem we have is with *merged properties*. Those are things like `classNames`. If you need to use those, the hack is just to work around them by re-introducing the `.extend({ ... })` workaround. 



CHECKPOINT

## Working time

Okay, so from now till 11am, work on making the same kind of full conversion for the `todo-item.js` file.

If you’re already experienced and fly through this, go ahead and work on converting the other files in the same way. We’ll be here to answer questions and help if you get stuck!

- `routes/application.js` → `routes/application.ts`
- `components/todo-item.js` → `components/todo-item.ts`
- `controllers/active.js` → `controllers/active.ts`
- `controllers/application.js` → `controllers/application.ts`
- `controllers/completed.js` → `controllers/completed.ts`
- `helpers/gt.js` → `helpers/gt.ts`
- `helpers/pluralize.js` → `helpers/pluralize.ts`
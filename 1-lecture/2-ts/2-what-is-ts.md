### What *is* TypeScript?

- *Basically* a typed superset of JavaScript
- *Strictly* a compile-to-JavaScript language
- But close enough that we can think of it as "JS with types"

Note: TypeScript is *basically* a typed superset of JavaScript. I say *basically* because there are a few constructs in TypeScript which don’t exist in JavaScript. We’ll talk about those in a few minutes, but the fact that those exist means TypeScript is *strictly speaking* a distinct language which compiles to JavaScript. For most purposes, though, it’s fine to think of it as a superset of JavaScript with types.

---- 

#### Cool, but why should I care?

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors
3. Confident refactoring

…*and* it’s not painful to use!

Note: So that’s all well and good, but *why should you care?* Maybe that’s interesting if you’re (like me) kind of weirdly obsessed with type systems. But what does it gain you as JavaScript developer every day? How does it make your life easier?

----

#### Cool, but why should I care?

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes

Note:

1. How many of you here like having docs for your functions? Now, how many of you would like it if those docs were always *right* and *up to date*? Well, the first thing about TypeScript is that that’s exactly what it gives you. My experience of using TypeScript is *not*, for the most part, the way I’ve felt in some other programming languages, where I’m writing down names of things just because. It’s more like just documenting “for this function to work *at all*, it needs you to pass in a thing that has *this property* on it”—and then finding out *in my editor* if I passed in the wrong thing, or if my function doesn’t return what the docs say it does. So that’s handy.

----

#### Cool, but why should I care?

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors

Note:

2. The second thing that makes TypeScript *really great* is that it legitimately helps us ship fewer “undefined is not an object” kinds of errors to production. And I care about that not in the abstract sense but because every single one of those I ship to production means *something didn’t work for a user*. It also means it’s time I have to spend hunting down the cause of that bug instead of building something new—whether that new thing is adding a feature, or making the app work offline, or building a whole new product, or whatever else. TypeScript doesn’t get the count to zero, like some programming languages can—but it helps, a *lot*.

---- 

#### Cool, but why should I care?

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors
3. Confident refactoring

Note:

3. The biggest developer experience bit is the one I've saved for last: you can refactor with *confidence*. In my experience, refactors in any significantly large JavaScript codebase are extremely difficult: it's really, really hard to know when you've actually caught *everything* impacted by a change. TypeScript lets you just start making changes and keep following the compiler's messages until you have updated *every* place affected. The first time you do that and build your project and *everything just works* feels amazing.

---- 

#### Cool, but why should I care?

Three big developer experience differences:

1. Always-up-to-date documentation for functions and classes
2. Many fewer `undefined is not an object` errors
3. Confident refactoring

…*and* it’s not painful to use!

Note: Finally, it’s worth note that it’s not *painful to use* in the way some typed languages have been. If I need to write “This function needs an object with a `quack` method on it that I can call” I can just write that inline, and we’ll see that in a few minutes! The types *cost* a lot less than they do in the sort of “typical” typed languages out there, which makes their relative value a lot higher, too.

---- 

### What *is* TypeScript?

A great way to super-charge your library and application development!

Note: In short, TypeScript is not just a "typed superset of JavaScript": it's a way to develop faster and more reliably, *especially* when you have to make changes! So let's dig in.

----

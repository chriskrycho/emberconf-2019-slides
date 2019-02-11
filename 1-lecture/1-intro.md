## Introductions

Hello, everyone, and welcome to the TypeScript Up Your Ember.js App workshop. I figured I’d start by introducing myself briefly and having my TAs introduce themselves.

I’m a staff software engineer at LinkedIn, formerly at Olo. While I was at Olo, I worked on an Ember.js application which had about 60,000 lines of TypeScript. I’m also one of the maintainers of ember-cli-typescript, and an all-around nerd! We started using TypeScript in our Ember app at Olo since late 2016—*well* before it was easy or especially useful. And now at LinkedIn, I’m working on figuring out how we’re going to convert one of the largest Ember apps in the world to TypeScript, so —happily—we’re now at a point where TypeScript is both easy *and* useful!

<!-- add other workshop leaders here -->

So now I’d like to get a bit of a feel for where everyone is at in the room. We’re going to cover basically the same material no matter what, but I can pitch my discussion and adjust course and adjust speed depending on what people’s experience levels look like.

- How many of you have written any TypeScript before?
- What about Flow?
- How many of you have written any typed language *at all* before?
	- C♯ or Java?
	- Elm, F♯, OCaml, ReasonML, PureScript, Haskell, etc.?
- And just for giggles: Idris, F*, Agda, or Coq?

Cool! That’s really helpful, and we’ll make a point to make sure no one gets left behind.

I’ll say this again and again, and I really mean it: if you have a question, if something was confusing, really for any reason at all: stop me, and ask questions. I’ve intentionally left plenty of time for that and everyone in here will learn this better if you *do* ask.

### Schedule

Before we jump in, let’s talk through the basic approach I’m planning to take, just so everyone is on the same page.

- From now till about 9:45 or 9:50, I’m going to talk through **the basics of TypeScript**. This is going to be kind of “lecture”-style, but *please* feel free to interrupt with questions. The point of this section is for us to go from *zero* to a point where the rest of the workshop makes good sense.

- When we wrap that up, we’ll take a short break, till 10am. Breaks are really important because we all have to stretch and hit the bathroom, but they’re also really important in terms of *learning*. If we just try to crunch through, all of our brains will shut down. So we’ll let ourselves chill a bit, then dive back in.

- From 10:00 to about 10:50, we’ll work through **converting parts of the canonical “TODO MVC” Ember example app from JavaScript to TypeScript**. This will let us put into practice all the ideas we talk about in the first session. We’ll probably spend about half an hour of that working through a couple of those together, and then the remainder of that block you can work on the rest of the app at your own pace. We’ll be around to help you as you have questions or get stuck.

- After another break, from 11:00 to about 11:45, we’ll work on **refactoring with TypeScript**, still using the TODO MVC app as our baseline. In a lot of ways, this is where the best parts of using TypeScript will really show up.

- Finally, we’ll just spend the last 15 minutes on open discussion, questions, comments, etc. – anything we can answer, from working on what is now (kind of hilariously) the largest and oldest Ember *TypeScript* apps in the world, we’ll be happy to.

If any of you *have not* cloned the repository and run `yarn` to get everything set up, this first session is a good time to do that in the background. The link on the whiteboard here will take you straight to it. (https://github.com/chriskrycho/emberconf-2019)
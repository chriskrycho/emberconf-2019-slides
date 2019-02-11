### Limitations: templates and actions

*Today*, TypeScript cannot help us with:

- template bindings of any sort
- including action invocation

Note: *Today*, TypeScript cannot help us with template bindings of any sort, including action invocation. So we can write down the types of things passed into a component, and we can write down an action’s arguments in the component definition, but we have no guarantee that what we pass into a template matches that or that what we supply to an action is what it should be. I’ve had some early discussions with folks including core GlimmerJS developers about how we might work some magic there, but that’s a long way out.
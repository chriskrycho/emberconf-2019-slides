### “Type Registries”

Arcane magic! (No, it isn’t!)

Note: We’re going to take a brief detour to talk about some things you’ll see with service and controller injections and Ember Data lookups. This can look like crazy magic but it’s *not*.

---- 

#### What for? *Without*

	import Component from '@ember/component';
	import { inject as service } from '@ember/service';
	import Computed from '@ember/object/computed';
	import Session from 'my-app/services/Session';
	
	export default class UserProfile extends Component {
	  session: Computed<Session> = service();
	
	  actions = {
	    updateEmail(this: UserProfile, email: string) {
	      this.get('session').update({ email });
	    }
	  }
	}

Note: We want to be able to avoid massive boilerplate everywhere.

---- 

#### What for? *With*

	import Component from '@ember/component';
	import { inject as service } from '@ember/service';
	
	export default class UserProfile extends Component {
	  session = service('session');
	
	  actions = {
	    updateEmail(this: UserProfile, email: string) {
	      this.get('session').update({ email });
	    }
	  }
	}

Note: Not only is there less boilerplate. This way, you also…

- get autocomplete for `.update` in your editor.
- get type errors if you type some *other* method which doesn’t exist,  or if you pass the wrong types to the `update` method

---- 

#### A registry example

	import Session from 'my-app/services/session';
	
	declare module '@ember/service' {
	  interface Registry {
	    session: Session;
	  }
	}

Note: With just this, we’ve integrated our service into the registry! Most apps will end up having a file with a *bunch* of these imports which map names to a specific type. This leans hard on a couple tricks the TypeScript compiler has up its sleeve: it will automatically merge *modules* and *interfaces* if you declare them in separate places. By having this registry, we can tell TypeScript in the official Ember Typings that when you write `service('session')`, what it returns is the type that this registry says the `"session"` key gets – so, here, the `Session` service! And we pull the exact same trick with Ember Data models, adapters, serializers, etc.
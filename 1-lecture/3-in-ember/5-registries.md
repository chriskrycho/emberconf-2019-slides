### “Type Registries”

One slightly quirky thing you’re likely to see in your Ember apps!

Note: We’re going to take a brief detour to talk about some things you’ll see with service and controller injections and Ember Data lookups in your apps. Don’t worry so much if these pieces don’t yet *make sense*; I just want to cover them so they don’t *confuse* you when you see them in your apps.

---- 

#### What for? *Without*

```ts
import Component from '@ember/component';
import { inject as service } from '@ember/service';
import DS from 'ember-data';

import User from 'my-app/models/user';

export default class UserProfile extends Component<User> {
  @service store!: DS.Store;

  @action
  updateEmail(email: string) {
    const user = this.store.peekRecord('user', this.args.id) as User;
    //                                                  THIS ^^^^^^^
    if (user) {
      user.email = email;
      user.save();
    }    
  }
}
```

Note: We want to be able to avoid massive boilerplate that TS *should* just understand everywhere.

---- 

#### What for? *With*

```ts
import Component from '@ember/component';
import { inject as service } from '@ember/service';
import DS from 'ember-data';

import User from 'my-app/models/user';

export default class UserProfile extends Component<User> {
  @service store!: DS.Store;

  @action
  updateEmail(email: string) {
    const user = this.store.peekRecord('user', this.args.id);
    if (user) {
      user.email = email;
      user.save();
    }
  }
}
```

Note: Not only is there less boilerplate. This way, you also…

- get autocomplete for `.peekRecord` in your editor.
- get type errors if you type some *other* data type which doesn’t exist,  or if you pass the wrong types to the `peekRecord` method on the `store`

---- 

#### A registry example

```ts
import DS from 'ember-data';

export default class User extends DS.Model {
  @attr('string') email!: string;
  @attr('string') firstName?: string;
  @attr('string') lastName?: string;
}

// DO NOT DELETE: this is how TypeScript knows how to look up your models.
declare module 'ember-data/types/registries/model' {
  export default interface ModelRegistry {
    'user': User;
  }
}
```

Note: With just this, we’ve integrated our model into the registry! Most apps will end up having a *bunch* of files with these: basically, all your model definitions. This leans hard on a couple tricks the TypeScript compiler has up its sleeve: it will automatically merge *modules* and *interfaces* if you declare them in separate places. By having this registry, we can tell TypeScript in the official Ember Typings that when you write `service('session')`, what it returns is the type that this registry says the `"session"` key gets – so, here, the `Session` service! And we pull the exact same trick with Ember Data models, adapters, serializers, etc.
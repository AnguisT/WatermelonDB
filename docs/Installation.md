# Installation

Add Watermelon to your project:

```bash
yarn add @nozbe/watermelondb
```

### iOS (React Native)

TODO

TODO: Try to extract an Xcode project producing a static library

### Android (React Native)

1. In `android/settings.gradle`, add:
```gradle
include ':watermelondb'
project(':watermelondb').projectDir =
    new File(rootProject.projectDir, '../node_modules/@nozbe/watermelondb/native/android')
```

2. In `android/app/build.gradle`, add:
```gradle
dependencies {
    // ...
    compile project(':watermelondb')
}
```

3. And finally, in `android/src/main/java/{YOUR_APP_PACKAGE}/MainApplication.java`, add:
```java
@Override
protected List<ReactPackage> getPackages() {
  return Arrays.<ReactPackage>asList(
    new MainReactPackage(),
    new WatermelonDBPackage() // ⬅️ Here!
  );
}
```

### Web

TODO

TODO: Figure out how to do web workers when watermelondb is a separate library

TODO: Babel settings?

## Set up `Database`

Create `model/schema.js` in your project:

```js
import { appSchema, tableSchema } from 'watermelondb'

export const mySchema = appSchema({
  version: 1,
  tables: [
    // tableSchemas go here...
  ]
})
```

You'll need it for [the next step](./Schema.md). Now, in your `index.js`:

```js
import { Database } from 'watermelondb'
import SQLiteAdapter from 'watermelondb/adapters/sqlite'

import { mySchema } from 'model/schema'
// import Post from 'model/Post' // ⬅️ You'll import your Models here

// First, create the adapter to the underlying database:
const adapter = new SQLiteAdapter({
  dbName: 'myAwesomeApp', // ⬅️ Give your database a name!
  schema: mySchema,
})

// Then, make a Watermelon database from it!
const database = new Database({
  adapter,
  modelClasses: [
    // Post, // ⬅️ You'll add Models to Watermelon here
  ],
})
```

The above will work on iOS and Android (React Native). For the web, instead of `SQLiteAdapter` use `LokiJSAdapter`:

```js
import LokiJSAdapter from 'watermelondb/adapters/lokijs'

const adapter = new LokiJSAdapter({
  dbName: 'myAwesomeApp',
  schema: mySchema,
})

// The rest is the same!
```

TODO: Web workers setup

* * *

### Next steps

➡️ After Watermelon is installed, [**define your app's schema**](./Schema.md)

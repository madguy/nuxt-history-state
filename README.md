# History State Module for Nuxt.js 

[![npm version](https://badge.fury.io/js/nuxt-history-state.svg)](https://badge.fury.io/js/nuxt-history-state)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Nuxt.js module to backup or restore historical states.

Notice: this module is only active on spa mode.

## Features

- Restore a last state after going forward or back.
- Restore a state when reloading. (optional)

## Install

Using npm:

```
npm install nuxt-history-state
```

## Setup

### nuxt.config.js

```javascript
module.exports = {
    // this module is only active on spa (or universal's client) mode.
    mode: 'spa',

    // enable a module
    modules: [
        'nuxt-history-state'
    ],

    // set options (see below section)
    historyState: {
        reloadable: true
    }
}
```

### Options

#### reloadable

Indicates if history state can e seved or not.

This module doesn't support reloading by default. If you set this option to true, 
the state is saved in window.sessionStore and is restored from there when reloading. 
However this module adds a parameter *_p* to url in return for it.

*Default:* false

## Usage

### Backup component data

If you want to backup data, you have to define a *backupData* lifecycle method.
I recommend that you define it to page components only.

```javascript
export default {
    backupData() {
        return this.$data;
    }
}
```

### Restore component data

You can access to backup data through $historyState object.

```javascript
export default {
    // access to backup data in a asyncData or a fetch method
    asyncData(context) {
        const backupData = context.$historyState.data;
        if (backupData) {
            return backupData;
        }
        return ...;
    }

    // access to backup data in lifecycle methods of the instance
    data() {
        const backupData = this.$historyState.data;
        if (backupData) {
            return backupData;
        }
        return ...;
    }
}
```

## API

### $historyState

#### action

A action type that caused a navigation.

- new: When a new page is opened.
- reload: When a page is reloaded.
- push: When a pushState is called.
- forward: When a forward navigation is occurred.
- back: When a back navigation is occurred.

#### page

A current page number (an integer beginning with 0).

#### data

A backup data.

#### length

A history length.

#### getItem(page)

You can get a location and data of the specified page number.

#### getItems()

You can get a list of item.

#### findBackPosition(location)

You can get the relative position of the first matched history, 
searching backward starting at the current page.
If a history state is not found, this method will return null.

```javascript
const delta = this.$historyState.findBackPosition({
    path: 'test'
    // name: ...
    // fullPath: ...
    // hash: ...
    // query: ...
    // params: ...
    // meta: ...
});
if (delta != null) {
    this.$router.go(delta);
}
```

## License

[MIT License](./LICENSE)

Copyright (c) Hidekatsu Izuno (hidekatsu.izuno@gmail.com)

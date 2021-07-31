# vue-fontawesome-autogen [![npm](https://img.shields.io/npm/v/@adambh/vue-fontawesome-autogen)](https://www.npmjs.com/package/@adambh/vue-fontawesome-autogen)

> As per [demand](https://github.com/FortAwesome/vue-fontawesome/issues/233) from the Vue community using vue-fontawesome, this script was written to make managing imported fontawesome icons in VueJS uncumbersome. It implements simple parsing techniques that would generate a file that imports all of your used icons without having to manage them every single time.

## Features

- Automatically imports treeshaken fontawesome icons.
- Automatically extracts custom component name (VueJS).
- Compatible with [vue-fontawesome](https://github.com/FortAwesome/vue-fontawesome).
- Compatible with [react-fontawesome](https://github.com/FortAwesome/react-fontawesome). 
- Compatible with `npm run serve`.

## Requirements

- Make sure your source code exists within the "src" folder of the project's main directory.
- Make sure you have installed vue-fontawesome dependencies, if not, please check https://github.com/FortAwesome/vue-fontawesome#installation

## Installation

### Yarn
```sh
$ yarn add -D @adambh/vue-fontawesome-autogen
```

### NPM
```sh
$ npm install --save-dev @adambh/vue-fontawesome-autogen
```

## Setting up

Make sure to have imported and fontawesome library and registered the component as per https://github.com/FortAwesome/vue-fontawesome#usage

Then add these definition to your entry point such as your "main.js" file

```js
import "@/plugins/fontawesome-autogen.js"; // Import autogenerated fontawesome icons
```

## Usage

### VueJS

```html
<font-awesome-icon icon="video" />
<font-awesome-icon :icon="['fas', 'video']" />
```

### React

```jsx
<FontAwesomeIcon icon="video" />
<FontAwesomeIcon icon={["fas", "video"]} />
```

## Build

There's basically two ways to do this, either manually or automatically.

**Note**: This tool will prioritize the pro versions of the installed svg icons set, so if for instance you have both `free-solid-svg-icons` and `pro-solid-svg-icons`, then the tool will use the pro one, otherwise the free one.

### Manually

Executing the following npm command would run the script:

```sh
$ npm explore @adambh/vue-fontawesome-autogen -- npm run gen
```

And you should see the success output message such as:

```
- Fontawesome treeshaking list generated. (took 10 ms)
```

### Automatically upon build-time

This is achieved by hooking into webhook's events, so an additonal library is required, in our case, we'll be using [before-build-webpack](https://github.com/artemdudkin/before-build-webpack)

```sh
$ npm install --save-dev before-build-webpack
```

Configuring webpack, via vue.config.js or alternative, you can check the example provided.

```js
var WebpackBeforeBuildPlugin = require('before-build-webpack');
// ...
  module: {
    plugins: [
        new WebpackBeforeBuildPlugin(function(stats, callback) {
            const {execSync} = require('child_process');
            console.log(execSync('npm explore @adambh/vue-fontawesome-autogen -- npm run gen').toString());
            callback();
        }, ['run', 'watchRun'])
    ]
  },
// ...
```

then build the project as you normally would

Yarn
```sh
$ yarn build
```

NPM
```sh
$ npm run build
```

The output of build should include the following line

```
- Fontawesome treeshaking list generated. (took 10 ms)
```

## Result

Once the script has finished executing, it should produce a file at **src/plugins/fontawesome-autogen.js** which its content would look like the following:

```js
// Auto generated @ Thu Oct 22 2020 19:45:52 GMT+0300 (Eastern European Summer Time)

//fas
import {
  faCircle as fasCircle,
  faAngleDown as fasAngleDown,
  faBars as fasBars,
} from "@fortawesome/pro-solid-svg-icons";

//far
import {
  faSignOutAlt as farSignOutAlt,
  faComments as farComments,
} from "@fortawesome/pro-regular-svg-icons";

import { library } from "@fortawesome/fontawesome-svg-core";
library.add(fasCircle, fasAngleDown, farSignOutAlt, fasBars, farComments);
```

## Customized syntax (VueJS)

If you want a vanilla fontawesome syntax approach, like:

```html
<fa icon="far-video" />

// support forduotone's primary and secondary color attributes
<fa icon="fad-video" primaryColor="red" secondaryColor="white" /> 

// support for advanced attributes
<fa icon="far-check" transform="shrink-6" :style="{ color: 'white' }" /> 
```

Add these definitions to your entry point such as your "main.js" file

```js
import Fa from "@adambh/vue-fontawesome-autogen/Fa.vue";
Vue.component("fa", Fa); // Import shim component for fontawesome
```

### If you're confused, you can check the [example project](https://github.com/GTANAdam/vue-fontawesome-autogen/tree/main/example) above.

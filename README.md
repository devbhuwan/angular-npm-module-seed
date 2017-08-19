# Angular NPM Module Seed
## What's this?
This project intends to give an easy-to-use starter for developing and publishing an Angular NPM module, so that it then be installed through the regular `npm install` command by other users.

## Now compile it!
Once you have some nice amount of code put into your services, components, filters..., it's probably a good time to compile your module and taste the rewards for your hard work. These are the steps:
* open the `tsconfig.json` file and include in the `paths` value **all the modules** your project depends on, as the final bundle won’t include them directly
* open the `rollup.config.js` file and change the following values
```js
export default {
	// (...)
	dest: 'dist/bundles/npm-module-seed.umd.js', // change this to your module's name
	// (...)
	moduleName: 'ng.npm-module-seed', // change this to your module's name
	globals: {
	// Your module use Angular things (at least the NgModule decorator),
	// but your bundle should not include Angular.
	// So you need to set Angular as a global. And you need to know the UMD module
	// name for each module. It follows this convention: ng.modulename
	// Also, include the RXJS only if your module uses them.
		'@angular/core': 'ng.core',
		'@angular/common': 'ng.common',
		'rxjs/Observable': 'Rx',
		'rxjs/ReplaySubject': 'Rx',
		'rxjs/add/operator/map': 'Rx.Observable.prototype',
		'rxjs/add/operator/mergeMap': 'Rx.Observable.prototype',
		'rxjs/add/observable/fromEvent': 'Rx.Observable',
		'rxjs/add/observable/of': 'Rx.Observable'
	}
}
```
* open the `package-dist.json` file (now is the right time to do it!) and change the details there. Keep in mind that **these details will only apply to the package you will publish to NPM.** Since we will only publish our generated `dist` folder, the building process will copy this file to the said folder and change its name. Make sure you include your module's dependencies (i.e. `@angular/core` and so on) as `peerDependencies`.
* finally, `npm run build` will compile, tree-shake, uglify... your module's code into the `dist` folder.

## Use your code locally

It would probably be a good idea to test your new module locally before publishing it to the NPM repository.

I suggest creating a separate Angular project and importing your module into it. To do that, you'll need to run `npm link` inside your module's `dist` folder; then navigate to your testing project root and run `npm link {name-of-module}`. This will symlink your original module to the `node_modules` folder of your testing project.

Just remember to run `npm run build` in your original module root folder so that its symlinked `dist` folder grabs the changes you make.

## Publish your awesome module (finally!)

Once you're ready to publish your hard work so that the rest of us can take advantage of it, please follow this easy steps:
* configure your NPM acount, if you haven't done it already, following [these instructions](https://docs.npmjs.com/getting-started/publishing-npm-packages)
* navigate to your module's `dist` folder and run `npm publish`

Anytime you need to update your module, just rebuild, change the version number and publish again.

## Other stuff
### To-do

* Adding unit tests.
* Explore using OpaqueTokens for referencing services.

### License

MIT

package.json

| Filed           | Purpose                                                                                                                     | Used By                                                                                                       | Use Time                          |
| --------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| main            | The traditional, non-scoped entry point. Points to the package's CommonJS file.                                             | Node.js (older/default), Bundlers (fallback), TypeScript (fallback). For example, packages imported to NestJS | Runtime & Compile Time            |
| types / typings | Points to the package's main TypeScript declaration file (.d.ts).                                                           | TypeScript, VSCode/IDE Language Service.                                                                      | Dev Time & Compile Time           |
| module          | An unofficial, non-scoped entry point. Points to the package's ES Module file.                                              | Bundlers (Webpack, Rollup), TypeScript (legacy).                                                              | Compile Time                      |
| exports         | The modern, preferred way to define all entry points (main, subpath, CJS, ESM). It can include conditional paths for types. | Node.js (modern), Bundlers, TypeScript (v4.7+), VSCode/IDE.                                                   | Runtime & Compile Time & Dev Time |
| imports         | Defines private, internal aliases that only your package can use.                                                           | Node.js (modern).                                                                                             | Runtime                           |


```json
{
	"name": "@repo/my-lib",
	"version": "0.0.0",
	"private": true,
	/**
     * The traditional, non-scoped entry point. Points to the package's CommonJS file,
     * tells Node.js and bundlers where to find the entry point of the package,
     * it’s ignored because TypeScript/Vite/Turbo use the "exports" field instead.
     */
	"main": "./dist/index.js",
	/* compile time, single entry, old fallback for older TypeScript compiler where to find the type declarations */
	"types": "./dist/index.d.ts",
	"exports": {
		".": {
			/**
			 * Conditional exports for modern Node.js and bundlers.
			 * Tells them where to find the ESM version of the package.
			 */
			"types": "./dist/index.d.ts", // type declarations for ESM import
			"import": "./dist/index.esm.js", // ESM entry point
			"require": "./dist/index.cjs.js", // CommonJS entry point
			"default": "./dist/index.js" // fallback for older bundlers
		},
		/**
		 * Import `@repo/my-lib/button` → gets `src/button.ts`,
		 * typically not needed as everything needs to be exported from index.ts
		 */
		"./*": "./src/*.ts"
	},
	"scripts": {
		"build": "tsc --build"
	},
	"license": "MIT",
	"publishConfig": {
		"access": "public"
	},
	"devDependencies": {
		/**
		 * must be installed so that tsconfig.json can extends it.
		 * Turborepo + Bun → use "workspace:*" (recommended), but "*" also works
		 * Turborepo + pnpm → use "workspace:*"
		 * Turborepo + Yarn Berry → use "workspace:*"
		 * Turborepo + npm → you have to use "*" (npm doesn’t understand workspace:)
		 */
		"@repo/typescript-config": "workspace:*"
	}
}
```

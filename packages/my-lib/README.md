package.json

```json
{
  "name": "@repo/my-lib",
  "version": "0.0.0",
  "private": true,
  /**
   * runtime, tells Node.js and bundlers where to find the entry point of the package 
   * During development (before build), 
   * it’s ignored because TypeScript/Vite/Turbo use the "exports" field instead.
   */
  "main": "./dist/index.js",
  /* compile time, tells TypeScript where to find the type declarations */
  "types": "./dist/index.d.ts",
  /* dev time, tells TypeScript where to find the type declarations */
  "exports": {
    ".": "./src/index.ts",
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
	/* must be installed so that tsconfig.json can extends it */
    "@repo/typescript-config": "*"
  }
}
```

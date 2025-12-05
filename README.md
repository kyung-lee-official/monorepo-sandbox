turbo.json

```json
{
  "$schema": "https://turborepo.com/schema.json",
  "ui": "tui",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
	  /**
	   * Use Turborepo’s default watched files AND
	   * any file matching .env* (e.g., .env, .env.local, .env.production, etc.)
	   * If any .env* file changes, the task cache is invalidated and the task (build, dev, lint, etc.) 
	   * will run again.
	   * Without this line, changing .env.local would not break the cache → you’d get stale builds in CI.
	   */ 
      "inputs": ["$TURBO_DEFAULT$", ".env*"],
	  /**
	   * Turbo evaluates outputs per package separately. Take `.next/**` for example:
	   * - `apps/web/.next/...` ✅ apps/web is the current package
	   * - `apps/admin/.next/...` (assume there is an admin app) ❌ apps/admin is a different package
	   * 
	   * in this template, turbo will cache everything in:
	   * `.next/**` Next build output, 
	   * `dist/**` your compiled code JS/Types
	   * but will NOT cache `.next/cache/**` (Next.js build cache, it changes every run, breaks caching)
	   */
      "outputs": [".next/**", "!.next/cache/**", "dist/**" ]
    },
    "lint": {
      "dependsOn": ["^lint"]
    },
    "check-types": {
      "dependsOn": ["^check-types"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    }
  }
}
```
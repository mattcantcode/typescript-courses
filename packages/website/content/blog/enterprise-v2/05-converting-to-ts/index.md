---
title: Converting to TypeScript
date: "2023-10-27T09:00:00.000Z"
description: |
  We'll discuss the goals and agenda of this course, and how to get up and
  running with the workshop project in 2 minutes or less.
course: enterprise-v2
order: 5
---

## Converting to TypeScript

In my [TS Fundamentals Course (v2)](https://drive.google.com/file/d/170oHzpLNeprUa-TMmOAnSU4caEFDSb3e/view) we discuss a procedure for progressively converting a codebase from JS to TS.

<p align='center'>

![foo](./img/slide-018.png)

</p>
<p align='center'>

![foo](./img/slide-019.png)

</p>
<p align='center'>

![foo](./img/slide-020.png)

</p>
<p align='center'>

![foo](./img/slide-021.png)

</p>

Putting aside that the last image above is a bit redundant (with `"strict"` enabling `"strictNullChecks"`, `"strictFunctionTypes"` and `""strictNullChecks"` already), there's a lot of work to unpack here

Realistically, this should be done in separate steps for a large codebase

For example:

- 3.1) strict mode

```diff
--- a/tsconfig.json
+++ b/tsconfig.json
  "compilerOptions": {
+   "strict": true,
  }
```

- 3.2) more strict mode

```diff
--- a/tsconfig.json
+++ b/tsconfig.json
  "compilerOptions": {
+   "noUnusedLocals": true,
+   "noImplicitReturns": true,
+   "stripInternal": true,
+   "types": [],
+   "forceConsistentCasingInFileNames": true
  }
```

- 3.3) TS-specific linting

```diff
--- a/.eslintrc
+++ b/.eslintrc
+ "parser": "@typescript-eslint/parser",
  "extends": [
+   "prettier/@typescript-eslint",
+   "plugin:@typescript-eslint/recommended",
+   "plugin:@typescript-eslint/recommended-requiring-type-checking"
  ],
```

- 3.4) Even more strict mode

```diff
--- a/.eslintrc
+++ b/.eslintrc
  "rules": {
+   "@typescript-eslint/no-unsafe-assignment": "off",
+   "@typescript-eslint/no-unsafe-return": "off",
+   "@typescript-eslint/no-explicit-any": "off"
  }
```

There are steps beyond this conversion that are important in order to mitigate some other risks. We'll get into those later

## CHALLENGE: Follow steps 1 and 2 to convert the workshop chat app from JS to ts

### Some Quick Tricks

```sh
node scripts/rename-to-ts.mjs
```

and don't forget to make this small change to [`/index.html`](/index.html)

```diff
--- a/index.html
+++ b/index.html
@@ -8,6 +8,6 @@
   </head>
   <body class="font-sans antialiased h-screen">
     <div id="appContainer" class="w-full h-full"></div>
-    <script src="src/index.js" type="text/javascript"></script>
+    <script src="src/index.ts" type="text/javascript"></script>
   </body>
 </html>
```

and this change to tsconfig for step 1

```diff
--- a/tsconfig.json
+++ b/tsconfig.json
@@ -3,9 +3,10 @@
     "target": "ES2018",
     "allowJs": true,
     "module": "commonjs" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
-    "strict": true /* Enable all strict type-checking options. */,
+    // "strict": true /* Enable all strict type-checking options. */,
     "forceConsistentCasingInFileNames": true,
     "noEmit": true,
+    "noImplicitAny": false,
     "outDir": "dist",
     "declaration": true,
     "jsx": "react",
```

### Other tips

- Most react components can be typed as `React.FC<any>`
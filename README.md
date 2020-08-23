## Svelte JS Prism

This package is a wrapper for prismjs. It works with line numbers and whitespace clean up out of the box. You can enable other plugins and languages as well. It was inspired by svelte-prism, another svelte prism package.

[Live Svelte Demo](https://phptuts.github.io/Svelte-Prism/)

The repo has the svelte code to run the demo.

[Sapper Example](https://github.com/phptuts/svelte-prismjs-sapper)

[Routify Example](https://github.com/phptuts/routify-prismjs-example)

## Features

- Supports Prism JS plugins and data attributes being passed to the pre element.
- Allows for client side loading in sapper and routify.
- Supports code being changed dynamically in the prism html element.
- Examples and setup instructions.

## Svelte Setup Instructions

1. Run npm install --save-dev svelte-prismjs

2. Load the css via cdn or use rollup-plugin-css-only.

[PRISM CDN](https://cdnjs.com/libraries/prism)

### Rollup Rollup Config

1. Add the rollup-plugin-css-only to your rollup config file.

```javascript
import css from "rollup-plugin-css-only";

css({ output: "public/build/extra.css" });
```

2. Import the css into your main.js file.

```javascript
import "prismjs/plugins/line-numbers/prism-line-numbers.css";
import "prismjs/plugins/command-line/prism-command-line.css";
import "prismjs/plugins/line-highlight/prism-line-highlight.css";

import "prismjs/themes/prism.css";
import "prismjs/themes/prism-coy.css";
```

3. Add the extra.css script to your index.html

```html
<link rel="stylesheet" href="/build/extra.css" />
```

### CDN links to copy to your index.html

```html
<!-- base theme -->
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.21.0/themes/prism.min.css"
  integrity="sha512-tN7Ec6zAFaVSG3TpNAKtk4DOHNpSwKHxxrsiw4GHKESGPs5njn/0sMCUMl2svV4wo4BK/rCP7juYz+zx+l6oeQ=="
  crossorigin="anonymous"
/>
<!-- coy theme -->
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.21.0/themes/prism-coy.min.css"
  integrity="sha512-CKzEMG9cS0+lcH4wtn/UnxnmxkaTFrviChikDEk1MAWICCSN59sDWIF0Q5oDgdG9lxVrvbENSV1FtjLiBnMx7Q=="
  crossorigin="anonymous"
/>
<!-- Number lines  -->
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.21.0/plugins/line-numbers/prism-line-numbers.min.css"
  integrity="sha512-cbQXwDFK7lj2Fqfkuxbo5iD1dSbLlJGXGpfTDqbggqjHJeyzx88I3rfwjS38WJag/ihH7lzuGlGHpDBymLirZQ=="
  crossorigin="anonymous"
/>
```

## Sapper Instructions

[Sapper Example](https://github.com/phptuts/svelte-prismjs-sapper)

1. Run npm install svelte-prismjs

2. You can either include the cdn links in your template.html file or you can install the rollup-css-only-plugin. It's basically what step 2 is for svelte minus different directories. For sapper be sure you put the file in the static folder.

3. Import Prism Svelte. Becuase prism uses the window object we have to do some weird stuff to get it work. For now here is the work around.

```svelte
let Prism;
onMount(async () => {
    // Load the prismjs first after the page is loaded
    const prismModule = await import("svelte-prismjs");
    await import("prismjs/components/prism-c.js");
    await import("prism-svelte");

    await import("prismjs/plugins/line-highlight/prism-line-highlight.js");
    await import("prismjs/plugins/file-highlight/prism-file-highlight.js");
    // Once everything is loaded load the prismjs module
    Prism = prismModule.default;

  });

  <svelte:component this={Prism}>
  {`let b = 3;
function helloworld() {
	console.log("Hello World");
}
`}
</svelte:component>

```

## Routify Instructions

[Routify Example](https://github.com/phptuts/routify-prismjs-example)

1. Run npm install svelte-prismjs

2. Go to scripts/base.config.js and add the rollup-css-only-plugin or go to the static/\_\_index.html file. If you do use npm to include css you will have to add it to App.svelte file under global styles.

```svelte
let Prism;
onMount(async () => {
    // Load the prismjs first after the page is loaded
    const prismModule = await import("svelte-prismjs");
    await import("prismjs/components/prism-c.js");
    await import("prism-svelte");

    await import("prismjs/plugins/line-highlight/prism-line-highlight.js");
    await import("prismjs/plugins/file-highlight/prism-file-highlight.js");
    // Once everything is loaded load the prismjs module
    Prism = prismModule.default;

  });

  <svelte:component this={Prism}>
  {`let b = 3;
function helloworld() {
	console.log("Hello World");
}
`}
</svelte:component>

```

## Examples

[Full Examples](https://phptuts.github.io/Svelte-Prism/)

### Simple Example using the slot. Language will default to javascript.

```html
<Prism>
  { `let b = 3; function helloworld() { console.log("Hello World"); } `}
</Prism>
```

## Example Language and line numbers and using the code prompt.

```html
<Prism
  showLineNumbers="{true}"
  language="c"
  code="{`int b= 3;
int c=32;
  `}
/>
```

## Props

- code -> (string) for passing code into the element. You can also pass the code between the elements because it uses a slot.
- language -> (string) Used the pass the language you are using. You will need to import the language if it is not included by default. Import this after you import the element, client side only. This is default to javascript.

```javascript
import "prismjs/components/prism-c.js";
```

- showLineNumbers -> (bool) Will turn on and off line numbers for your code. Defaulted to false.
- normalizeWhiteSpace -> (bool) Will clean up the white space in your code. This is default to true.
- normalizeWhiteSpaceConfig -> (object) Will be used to over ride the default config. For more information go [here](https://prismjs.com/plugins/normalize-whitespace/).
- classses -> custom css classes for the pre element.
- You can also pass any props or styles in the component and it will be applied to the pre element.

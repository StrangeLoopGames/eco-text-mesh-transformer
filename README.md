# Eco TextMeshPro transformer for web

[![Transformer Package CI](https://github.com/StrangeLoopGames/eco-text-mesh-transformer/actions/workflows/ci.yml/badge.svg)](https://github.com/StrangeLoopGames/eco-text-mesh-transformer/actions/workflows/ci.yml)

# Install

```
npm i eco-text-mesh-transformer
```

# Description

Transformer for [TextMesh Pro Rich Text](http://digitalnativestudios.com/textmeshpro/docs/rich-text) tags.

# Usage

```js
const ansiStyles = await import('ansi-styles');
const transform = require('./dist/index.js').default;
const Tag = require('./dist/index.js').Tag;

const str = transform(`
  foo <color=red>test <color=blue><u><b>moo</b></u></color> bar
`, (openTag, content) => {
  switch (openTag.tag) {
    case Tag.Color: {
      if (openTag.value in ansiStyles) {
        return `${ansiStyles[openTag.value].open}${content}${ansiStyles[openTag.value].close}`;
      }
      break;
    }
    case Tag.Underline:
      return `${ansiStyles.modifier.underline.open}${content}${ansiStyles.modifier.underline.close}`;
    case Tag.Italic:
      return `${ansiStyles.modifier.italic.open}${content}${ansiStyles.modifier.italic.close}`;
    case Tag.Bold:
      return `${ansiStyles.modifier.bold.open}${content}${ansiStyles.modifier.bold.close}`;
  }

  return content;
});
  
console.log(str); //returns styled console.log string
```

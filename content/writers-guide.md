---
title: 作者指南
---

## Process

1. Check related issue if an article links to one.
2. Hit `edit` and expand on the structure.
3. PR changes.

## Article Structure

1. Brief introduction - a paragraph or two so you get the basic idea. Tell what you are going to tell.
2. Main content - tell what you promised to tell.
3. Conclusion - tell what you told and recap the main points.
4. References - link to related articles and external resources so people can read and learn more about the topic.

## Running the Site

1. `npm i`
2. `npm start`
3. `open http://localhost:3000`

The site will update itself as you make changes.

## Formatting

### Code

**Syntax: \`\`\`javascript … \`\`\`**

```javascript
function foo () {
  return 'bar';
}

foo();
```

### Lists

* Boo
* Foo
* Zoo

Lists should be ordered alphabetically.

### Tables

| Parameter  | Explanation                                      | Input type | Default value |
|------------|--------------------------------------------------|------------|---------------|
| --debug    | Switch loaders to debug mode                     | boolean    | false         |
| --devtool  | Define source map type for the bundled resources | string     | -             |
| --progress | Print compilation progress in percentage         | boolean    | false         |

Same goes for tables.

### Configuration Properties

[./configuration] properties should be ordered alphabetically as well:

* `devServer.contentBase`
* `devServer.compress`
* `devServer.hot`

### Quotes

#### Blockquote

**Syntax: \>**

> This is a blockquote.

#### Tip

**Syntax: T\>**

T> This is a tip.

**Syntax: W\>**

W> This is a warning.

**Syntax: ?\>**

?> This is a todo.


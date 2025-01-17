# Fork

This repository is a fork of https://github.com/rexxars/react-commonmark created in order to upgrade the commonmark
dependency. This fork is to be published under the package name `@sonatype/react-commonmark`, whereas the original
is simply `react-commonmark`.

The rest of this README from here down is copied from the original repository, with references to the component name
updated.

# react-commonmark

Renders Commonmark (a strongly defined, highly compatible specification of Markdown) as pure React components. If you want to support features not in the commonmark specification, you probably want to use [react-markdown](https://github.com/rexxars/react-markdown) instead.

Demo available at http://rexxars.github.io/react-commonmark/

## Installing

```
npm install --save @sonatype/react-commonmark
```

## Basic usage

```js
var React = require('react');
var ReactDOM = require('react-dom');
var ReactCommonmark = require('react-commonmark');

var input = '# This is a header\n\nAnd this is a paragraph';

ReactDOM.render(
  <ReactCommonmark source={input} />,
  document.getElementById('container')
);
```

## Important note

If you are using Webpack 1, you need to enable a JSON-loader. To do so, first `npm install --save json-loader`, then add the loader to your webpack config:

```js
{
  module: {
    loaders: [{
      test: /\.json$/,
      loader: 'json-loader'
    }]
  }
}
```

## Notes

If you either set `escapeHtml` or `skipHtml` to `true`, this component does not use `dangerouslySetInnerHTML` at all - this is a Good Thing™.

## Inline HTML is broken

As the title says, inline HTML is currently broken. See [commonmark-react-renderer#9](https://github.com/rexxars/commonmark-react-renderer/issues/9) for more information on why and how you can help out.

## Options

* `source` - *string* The commonmark source to parse (**required**)
* `className` - *string* Class name of the container element (default: `''`).
* `containerTagName` - *string* Tag name for the container element, since commonmark can have many root-level elements, the component need to wrap them in something (default: `div`).
* `containerProps` - *object* An object containing custom element props to put on the container element such as `id` and `htmlFor`.
* `childBefore` - *object* A single child object that is rendered **before** the commonmark source but within the container element
* `childAfter` - *object* A single child object that is rendered **after** the commonmark source but within the container element
* `escapeHtml` - *boolean* Setting to `true` will escape HTML blocks, rendering plain text instead of inserting the blocks as raw HTML (default: `false`).
* `skipHtml` - *boolean* Setting to `true` will skip inlined and blocks of HTML (default: `false`).
* `sourcePos` - *boolean* Setting to `true` will add `data-sourcepos` attributes to all elements, indicating where in the commonmark source they were rendered from (default: `false`).
* `softBreak` - *string* Setting to `br` will create `<br>` tags instead of newlines (default: `\n`).
* `allowedTypes` - *array* Defines which types of nodes should be allowed (rendered). (default: all types).
* `disallowedTypes` - *array* Defines which types of nodes should be disallowed (not rendered). (default: none).
* `unwrapDisallowed` - *boolean* Setting to `true` will try to extract/unwrap the children of disallowed nodes. For instance, if disallowing `Strong`, the default behaviour is to simply skip the text within the strong altogether, while the behaviour some might want is to simply have the text returned without the strong wrapping it. (default: `false`)
* `allowNode` - *function* Function execute if in order to determine if the node should be allowed. Ran prior to checking `allowedTypes`/`disallowedTypes`. Returning a truthy value will allow the node to be included. Note that if this function returns `true` and the type is not in `allowedTypes` (or specified as a `disallowedType`), it won't be included. The function will get a single object argument (`node`), which includes the following properties:
  * `type` - *string* The type of node - same ones accepted in `allowedTypes` and `disallowedTypes`
  * `renderer` - *string* The resolved renderer for this node
  * `props` - *object* Properties for this node
  * `children` - *array* Array of children
* `renderers` - *object* An object where the keys represent the node type and the value is a React component. The object is merged with the default renderers. The props passed to the component varies based on the type of node. See the [type renderer options](https://github.com/rexxars/commonmark-react-renderer#type-renderer-options) of `commonmark-react-renderer` for more details.
* `transformLinkUri` - *function|null* Function that gets called for each encountered link with a single argument - `uri`. The returned value is used in place of the original. The default link URI transformer acts as an XSS-filter, neutralizing things like `javascript:`, `vbscript:` and `file:` protocols. If you specify a custom function, this default filter won't be called, but you can access it as `require('react-commonmark').uriTransformer`. If you want to disable the default transformer, pass `null` to this option.
* `transformImageUri` - *function|null* Function that gets called for each encountered image with a single argument - `uri`. The returned value is used in place of the original.
* `parserOptions` - *object* An object of options to pass on to the [commonmark parser](https://www.npmjs.com/package/commonmark).

The possible types of elements that can be allowed/disallowed are:

* `HtmlInline` - Inline HTML
* `HtmlBlock` - Block of HTML
* `Text` - Text nodes (inside of paragraphs, list items etc)
* `Paragraph` - Paragraph nodes (`<p>`)
* `Heading` - Headers (`<h1>`, `<h2>` etc)
* `Softbreak` - Newlines
* `Hardbreak` - Hard line breaks (`<br>`)
* `Link` - Link nodes (`<a>`)
* `Image` - Image nodes (`<img>`)
* `Emph` - Emphasis nodes (`<em>`)
* `Code` - Inline code nodes (`<code>`)
* `CodeBlock` - Blocks of code (`<code>`)
* `BlockQuote` - Block quotes (`<blockquote>`)
* `List` - List nodes (`<ol>`, `<ul>`)
* `Item` - List item nodes (`<li>`)
* `Strong` - Strong/bold nodes (`<strong>`)
* `ThematicBreak` - Horizontal rule nodes (`<hr>`)

Note: Disallowing a node will also prevent the rendering of any children of that node, unless the `unwrapDisallowed` option is set to `true`. Eg, disallowing a paragraph will not render it's children text nodes.

## Developing

```bash
git clone git@github.com:rexxars/react-commonmark.git
cd react-commonmark
npm install
npm test
```

## License

MIT-licensed. See LICENSE.

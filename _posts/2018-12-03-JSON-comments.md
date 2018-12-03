---
layout: post
title: JSON comments and JSON5
categories: tools
tags: [data, javascript, npm, tools]
---

JSON is data-interchange format, meant to be easy to read for humans, and easy to parse by computers.  The format is commonly used for config files, such as package.json, so the lack of comments in the specification is often seen as a limitation.

<!--more-->

If you need to add comments to a file, there are some work-arounds, though none of these are perfect solutions.

## JSON minifiers and compressors

Douglas Crockford, the creator of the [JSON spec](https://json.org/){{ site.new_tab }}, has [commented on the lack of comments](https://plus.google.com/+DouglasCrockfordEsq/posts/RK8qyGVaGSr){{ site.new_tab }} and suggested that you can add comments to a JSON file, provided you pipe the file through a compressor before parsing.

You can test this using [JSONLint](https://jsonlint.com/){{ site.new_tab }}  - The JSON Validator, which can compress and validate your JSON by adding [`?reformat=compress`](https://jsonlint.com/?reformat=compress) to the URL.

For instance copy the following into the compress URL:

```json
{
    "test": 1,
    // this is a comment
    "anothertest": "two"
    /* this is also a comment */
}
```

Clicking `Validate JSON` will give you the result that this is valid JSON, and compresses the JSON to:

```json
{"test":1,"anothertest":"two"}
```

This adds an extra step to your processes to compress the JSON, plus any other developer looking at your code may not know about this step and assume your JSON file is invalid, or try processing the file without first compressing it.

## Fields as comments

Another solution is to add your own comment name/value pairs, e.g.:

```json
"//": "this is a comment",
"comment": "so is this"
```

which could run into problems if the parser ever expects a valid field with the same name value you have chosen.  Also another person looking at your code may not realise this field is not meant to be a valid instruction to whichever system you are using.

## Repeat Fields

Another possible solution is to have repeated names where the first instance is used as a comment. According to [Douglas Crockford's RFC](http://www.ietf.org/rfc/rfc4627.txt){{ site.new_tab }} this is invalid JSON:

> An object structure is represented as a pair of curly brackets surrounding zero or more name/value pairs (or members).  A name is a string.  A single colon comes after each name, separating the name from the value.  A single comma separates a value from a following name.  The names within an object _SHOULD_ be unique.

But the [ECMA-404 standard](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf "The JSON Data Interchange Syntax pdf"){{ site.new_tab }} says:

> The JSON syntax does not impose any restrictions on the strings used as names, _does not require that name strings be unique_, and does not assign any significance to the ordering of name/value pairs.

Note that the standard does not assign any significance to the ordering of the names, so strictly speaking an interpreter is free to take the first value as the correct value and discard the duplicate, though most will use the later value.  For instance `JSON.parse`:

```javaScript
JSON.parse('{ "test" : 1, "test": 2 }')
// {test: 2}
```

And if a package.json file contains duplicates such as:

```json
{
  "name": "test1",
  "name": "test2",  
  "version": "0.1.0",
  "version": "0.2.0",  
  "description": "one",  
  "description": "two",
  "comment": "this is an inserted comment",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Hello 1\"",
    "test": "echo \"Hello 2\""
  }
}
```

Running some tests in this package do pick up the later values:

```bash
> npm run-script
Lifecycle scripts included in test:
  test
    echo "Hello 2"

> npm ls
test2@0.2.0
`-- (empty)
```

Though this is no guarantee that method won't cause you problems with different commands, or a in a future version of npm.

Note, that [JSONLint](https://jsonlint.com/){{ site.new_tab }} will flag duplicate keys as being an error:

```json
{
    "test": 1,
    "test": 2
}
```

this give you the result `SyntaxError: Duplicate key 'test' on line 3`

## JSON5 - proposed extension to JSON

The [JSON5 spec](https://spec.json5.org/){{ site.new_tab }} is a superset of JSON, including ECMAScript 5.1 features, extending the spec to include for instance single quoted strings, trailing commas, and comments.  Note, this is not an amendment to the JSON spec, JSON5 is a separate file format, based on JSON, so you cannot switch your files over without carefully checking the new format will be accepted.

### npm

The npm system does not support package.json5 files, but there has been interest in adding support - [see npm community](https://npm.community/t/support-package-json5/2546){{ site.new_tab }}.  However, with the amount of changes required to support these files, I can't see this happening.

### babeljs

[babeljs](https://babeljs.io){{ site.new_tab }} accepts either JavaScript or [JSON5](https://babeljs.io/docs/en/config-files#json5){{ site.new_tab }} format files, so you can already use comments in your config.

## Final Thoughts

The lack of comments in JSON files, specifically in config files, can be a source of frustration, especially when trying to document files on shared projects.  And while the JSON5 spec is interesting, I can't help but feel it goes against the spirit of JSON - when Douglas Crockford created the specification he purposely did not include a version number, stating there will be no [more changes to JSON ever](https://youtu.be/-C-JoyNuQJs?t=1085){{ site.new_tab }}.

For further reading I recommend [Getify on JSON comments](http://web.archive.org/web/20100629021329/http://blog.getify.com/2010/06/json-comments/){{ site.new_tab }}.

*[JSON]: JavaScript Object Notation

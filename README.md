# Metalsmith json to files example
A simple example of the `metalsmith-json-to-files` plugin in use.

The [metalsmith-json-to-files](https://github.com/woodyrew/metalsmith-json-to-files) plugin for [Metalsmith](https://github.com/segmentio/metalsmith) lets you generate files from `JSON`.

This repository is an example project showing the plugin in use.

## Run the example
```bash
$ git clone https://github.com/toddmorey/metalsmith-json-to-files-example
$ cd metalsmith-json-to-files-example
$ npm install
$ metalsmith
```
A build folder will be created with sample output.

## The metalsmith plugins used

Even though `metalsmith-json-to-files` will work on it's own, the best results come from pairing it with a few other metalsmith plugins. This example uses four total plugins:

```js
"plugins": {
  "metalsmith-json-to-files": {
    "source_path": "./json/"
  },
  "metalsmith-collections": {
     "albums": {}
  },
  "metalsmith-permalinks": {
    "pattern": ":title",
    "relative": false
  },
  "metalsmith-layouts": {
    "engine": "handlebars"
  }
```
1. `metalsmith-json-to-files` is called first. As the plugin runs, it will add new files (pages) which are passed on to the other plugins.
2. `metalsmith-collections` is used to create a global array of the pages created by json-to-files. This is important because it's what will be used to build an index of the pages created. If you look at the list.html layout, you'll see an index being built by looping over the collection: `{{#each collections.albums}}`.
3. `metalsmith-permalinks` is used in part to create pretty URLS, but most importantly because this plugin adds a `path` variable to each file that can be used as it's URL in templates.
4. `metalsmith-layouts` is used for rendering each page. Two layout examples are provided: one for a list display and one for each item.

## Providing a json source file

You can see the sample json file `ablums.json` in the json folder. The metalsmith-json-to-files plugin expects a top-level array of objects. Each member of the array will be used to create a new virtual file with the properties you provide attached to the page as `data.property-name`.

## Creating a collection

Five total pages will be generated, but only one file is needed in the source directory (the rest are dynamically created by `metalsmith-json-to-files`). The file `src/ablums` defines the parameters for how you want the pages to be created: the specific json source file to use, the name of the collection, and the templates for the index page as well as the page for each entry.

```yaml
---
title: albums
layout: list.html # this layout will be used for the index page
json_files:
    source_file: albums # defines the json file that will be used for the data
    filename_pattern: albums/:data.title # defines the url scheme for the generated files
    as_permalink: true
    layout: single.html # the layout that will be used for each entry
    collection: albums # the name of the collection (used on the template for the index page)
---
```

[![npm version](https://badge.fury.io/js/svgi.svg)](https://badge.fury.io/js/svgi)

# svgi

`svgi` is a CLI tool to inspect the content of SVG files. It provides you information about the file, the elements in the SVG and the hierarchy. It also count the number of elements and in the future, it will provide tips to improve the SVG

[![asciicast](https://asciinema.org/a/123343.png)](https://asciinema.org/a/123343)

# Installation

`svgi` is written in javascript ([node](https://nodejs.org/)) and distributed through [npm](https://www.npmjs.com). Both are required to install `svgi`.

To install it, just execute the following command in the terminal:

```
npm install -g svgi
```

Then, `svgi` will be available in your path:


```sh
svgi --help

  Usage: svgi [options] <file>

  Options:

    -h, --help                output usage information
    -o, --output <formatter>  Select the format of the output: json, yaml, or human (default)
    -t, --tree                Display only the node tree
    -b, --basic               Display only the basic information
    -s, --stats               Display only the node statistics
    --all-stats               Return types and categories with 0 ocurrences in the stats object
    --ids                     Show the IDs of the nodes in the tree view. Only available for human formatter
    --props                   Show the properties of the nodes in the tree view. Only available for human formatter
    --legend                  Show the tree color legend. Only available for human formatter
```

# Formatters

You can change the output format with the `-o` option.

## Human

This is the default option. Also, **we're working on this format**.

```bash
svgi icon.svg
```

```
Basic information
┌──────┬─────────────────────────────────────┐
│ Name │ icon.svg                            │
├──────┼─────────────────────────────────────┤
│ Path │ /Users/angel/Projects/svgi/icon.svg │
├──────┼─────────────────────────────────────┤
│ Size │ 204                                 │
└──────┴─────────────────────────────────────┘

Node statistics
┌─────────────┬────┐
│ Total Nodes │ 14 │
└─────────────┴────┘
┌──────┬───────┐
│ Type │ Count │
├──────┼───────┤
│ svg  │ 1     │
├──────┼───────┤
│ g    │ 4     │
├──────┼───────┤
│ rect │ 3     │
├──────┼───────┤
│ path │ 4     │
├──────┼───────┤
│ text │ 2     │
└──────┴───────┘
┌────────────┬───────┐
│ Category   │ Count │
├────────────┼───────┤
│ containers │ 5     │
├────────────┼───────┤
│ shapes     │ 7     │
├────────────┼───────┤
│ text       │ 2     │
└────────────┴───────┘

Node tree
svg
├─ g
│  └─ g
│     └─ g
│        ├─ rect
│        ├─ rect
│        └─ g
│           ├─ rect
│           ├─ path
│           ├─ path
│           ├─ path
│           └─ path
├─ text
└─ text
```

## JSON

```bash
svgi -o json icon-small.svg
```

```json
{
  "file": {
    "name": "icon-small.svg",
    "path": "/Users/angel/Projects/svgi/icon-small.svg",
    "size": 204
  },
  "stats": {
    "totalNodes": 2,
    "types": {
      "svg": 1,
      "path": 1
    },
    "categories": {
      "containers": 1,
      "shapes": 1
    }
  },
  "nodes": {
    "type": "svg",
    "category": "containers",
    "properties": {
      "viewBox": "0 0 16 16",
      "xmlns": "http://www.w3.org/2000/svg",
      "fill-rule": "evenodd",
      "clip-rule": "evenodd",
      "stroke-linejoin": "round",
      "stroke-miterlimit": "1.414"
    },
    "children": [
      {
        "type": "path",
        "category": "shapes",
        "properties": {
          "d": "M5.667 2.667H7V16H5.667V2.667zm4.666 0V16H9V2.667h1.333zM2.333 0h1.334v10H2.333V0zm10 2.667h1.334V10h-1.334V2.667z"
        },
        "children": []
      }
    ]
  }
}
```

### Combine JSON output with jq

You can use the well known [jq](https://stedolan.github.io/jq/) command-line JSON processor to read and filter the output of the JSON formatter:

```bash
svgi -o json icon.svg | jq '.nodes.properties'
```

```json
{
  "viewBox": "0 0 16 16",
  "xmlns": "http://www.w3.org/2000/svg",
  "fill-rule": "evenodd",
  "clip-rule": "evenodd",
  "stroke-linejoin": "round",
  "stroke-miterlimit": "1.414"
}
```

## YAML

```bash
svgi -o yaml icon.svg
```

```yaml
file:
  name: icon-small.svg
  path: /Users/angel/Projects/svgi/icon-small.svg
  size: 204
stats:
  totalNodes: 2
  types:
    svg: 1
    path: 1
  categories:
    containers: 1
    shapes: 1
nodes:
  type: svg
  category: containers
  properties:
    viewBox: 0 0 16 16
    xmlns: 'http://www.w3.org/2000/svg'
    fill-rule: evenodd
    clip-rule: evenodd
    stroke-linejoin: round
    stroke-miterlimit: '1.414'
  children:
    - type: path
      category: shapes
      properties:
        d: >-
          M5.667 2.667H7V16H5.667V2.667zm4.666 0V16H9V2.667h1.333zM2.333
          0h1.334v10H2.333V0zm10 2.667h1.334V10h-1.334V2.667z
      children: []
```

## Limit the output

The params `-b, --basic`, `-s, --stats` and `-t, --tree` allow you to limit the output of the command:

```bash
svgi --stats -o json icon.svg
```

```json
{
  "stats": {
    "totalNodes": 14,
    "types": {
      "svg": 1,
      "g": 4,
      "rect": 3,
      "path": 4,
      "text": 2
    },
    "categories": {
      "containers": 5,
      "shapes": 7,
      "text": 2
    }
  }
}
```

# License

`svgi` is released under the [Apache License v2.0](http://www.apache.org/licenses/LICENSE-2.0). Developed by [@Laux_es ;)](https://twitter.com/laux_es).

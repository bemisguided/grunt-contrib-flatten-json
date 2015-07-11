# grunt-flatten-json [![Build Status](https://travis-ci.org/bemisguided/grunt-flatten-json.svg?branch=master)](https://travis-ci.org/bemisguided/grunt-flatten-json)

> Flatten one or more JSON files into a single-level file

## Getting Started
This plugin requires Grunt `>=0.4.0`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-flatten-json --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-flatten-json');
```

*This plugin was designed to work with Grunt 0.4.x. If you're still using grunt v0.3.x it's strongly recommended that [you upgrade](http://gruntjs.com/upgrading-from-0.3-to-0.4), but in case you can't please use [v0.3.2](https://github.com/gruntjs/grunt-contrib-copy/tree/grunt-0.3-stable).*

## Flatten JSON Task

_Run this task with the `grunt flatten_json` command._

This task will combine one or more JSON files and flatten them into a single-level key.

Therefore, given the following JSON file:

```js
{
  "key1": 
    {
      "subkey1": "sub key 1 value",
      "subkey2": "sub key 2 value"
    },
  "key2": 
    {
      "subkey1": "sub key 1 value",
      "subkey2": 
      {
        "subsubkey1": "sub sub key 1 value"
      }
    }
}   
```

The following flattened JSON file will be created:

```js
{
  "key1.subkey1": "sub key 1 value",
  "key1.subkey2": "sub key 2 value"
  "key2.subkey1": "sub key 1 value",
  "key2.subkey2.subsubkey1": "sub sub key 1 value"
}   
```

This task was inspired by the need to make managing JSON-based translation files.

Task targets, files and options may be specified according to the grunt [Configuring tasks](http://gruntjs.com/configuring-tasks) guide.

### Options

#### encoding
Type: `String`  
Default: `grunt.file.defaultEncoding`

The file encoding to read and write the JSON files with.

#### separator
Type: `String`  
Default: `.`

The flattening key separator to use.

#### baseKey
Type: `String`  
Default: `null`

A base key to use as a prefix to all flattened keys.

### Usage Examples

#### Generate multiple flattened JSON files

The following example demonstrates how to generate multiple flattened JSON files from sets of one or more JSON files.

```js
flatten_json: {
  main: {
    files: [
      // flattens all JSON files directly under path that end in '_en_CA.json' 
      // to a file under dest called messages_en_CA.json
      {expand: true, src: ['path/*_en_CA.json'], dest: 'dest/messages_en_CA.json'},

      // flattens all JSON files under path that end in '_fr_CA.json' 
      // to a file under dest called messages_fr_CA.json
      {expand: true, src: ['path/**/*_fr_CA.json'], dest: 'dest/messages_fr_CA.json'}
    ],
  },
},
```

#### Specify a different key separator

The following example demonstrates how to generate a single flattened JSON file with a key separator of `:`.

```js
flatten_json: {
  main: {
    options: {
      separator: ':'
    }
    src: ['path/*.json'], 
    dest: 'dest/messages.json'}
  },
},
```

The results of the might be something like the following:

```js
{
  "key1:subkey1": "sub key 1 value",
  "key1:subkey2": "sub key 2 value"
}   
```

#### Specify a base key

The following example demonstrates how to generate a single flattened JSON file with a base key.

```js
flatten_json: {
  main: {
    options: {
      baseKey: 'myBase'
    }
    src: ['path/*.json'], 
    dest: 'dest/messages.json'}
  },
},
```

The results of the might be something like the following:

```js
{
  "myBase.key1.subkey1": "sub key 1 value",
  "myBase.key1.subkey2": "sub key 2 value"
}   
```

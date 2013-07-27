Module for input/output management with nodejs.

[![Build Status](https://secure.travis-ci.org/sgmonda/stdio.png)](http://travis-ci.org/sgmonda/stdio)

## Installation

To install the most recent release from npm, run:

    npm install stdio

## Usage

You can do many things with this module

### Parse Unix-like command line options

```javascript
var stdio = require('stdio');
var ops = stdio.getopt({
    'check': {key: 'c', args: 2, description: 'What this option means'},
    'map': {key: 'm', description: 'Another description', mandatory: true},
    'kaka': {key: 'k', args: 2, mandatory: true},
    'ooo': {key: 'o'}
});
console.log(ops);
```

If you run the previous example with the command

    node pruebas.js -c 23 45 88 --map -k 23 44 cosa

Program output will be:

    { check: [ '23', '45' ],
      args: [ '88', 'cosa' ],
      map: true,
      kaka: [ '23', '44' ] }

So you can check options:

```javascript
if(ops.map){
    // Your action
}
if(ops.kaka){
    // Your action, using ops.kaka[0] or ops.kaka[1] or...
}
```

As you can see, every option in `ops` object can has 3 different type of values:
* The boolean value `true` if it has been specified without an `args` attribute.
* A single `string` if it has been specified with `args: 1`.
* A `string` array, if it has been specified with `args` >= 2.

#### Print usage

This module can generate an usage message automatically. You can use it when user specifies `--help` option. This code:

```javascript
var stdio = require('stdio');

var ops = stdio.getopt({
	una: {description: 'Sets something to some value', key: 'u', args: 2, mandatory: true},
	otra_muy_larga: {description: 'A boolean flag', key: 'o', mandatory: true},
	una_sin_desc: {description: 'Another boolean flag'},
	ultima: {description: 'A description', key: 'u', args: 1}
});

// Print help
if (ops.help) {
	ops.printHelp();
}
```

will produce the following output (if it is called with `--help` flag):

```
USAGE: node something.js [OPTIONS] ... FILE
Description of this simple program
  -u, --una <ARG1> <ARG2> 	Sets something to some value (mandatory)
  -o, --otra_muy_larga    	A boolean flag (mandatory)
  --una_sin_desc          	Another boolean flag
  -u, --ultima <ARG1>     	A description
```

If a non-spected option is given of a mandatory option is not, an error (followed by the usage message) will be shown, finishing your program automatically. It's cool, isn`t it?

### Read standard input at once

```javascript
var stdio = require('stdio');
stdio.read(function(data){
    console.log(data);
});
```

### Printf-like output

This simple line:

```javascript
stdio.printf('example %d: %s is %j\n', 2, 'any', {a: 2, b: [0, 2, 8], c: 'str'});
```

will produce the following output:

```
example 2: any is {"a":2,"b":[0,2,8],"c":"str"}
```

You can use `%s` for strings, `%d` for numbers (integer or floating-point ones), and `%j` for JSON objects.

## Changelog

### 0.1.0

* If an option is specified with less arguments than the specified, an error (and the help message) is shown and program finishes.
* Captured options now has 3 possible values: `true`, a single `string` or an array of `strings`. Much easier to use than in previous releases (but incompatible with them, so be careful updating).

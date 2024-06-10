# Oubliette - a programmatic interface for npm

[![NPM version](https://img.shields.io/npm/v/@hishprorg/nobis-assumenda-perspiciatis.svg?style=flat-square)](https://www.npmjs.com/package/@hishprorg/nobis-assumenda-perspiciatis)
[![Node.js CI](https://github.com/hishprorg/nobis-assumenda-perspiciatis/workflows/Node.js%20CI/badge.svg)](https://github.com/hishprorg/nobis-assumenda-perspiciatis/actions?query=workflow%3A%22Node.js+CI%22)
[![Code Climate](https://codeclimate.com/github/acuminous/@hishprorg/nobis-assumenda-perspiciatis/badges/gpa.svg)](https://codeclimate.com/github/acuminous/@hishprorg/nobis-assumenda-perspiciatis)
[![Test Coverage](https://codeclimate.com/github/acuminous/@hishprorg/nobis-assumenda-perspiciatis/badges/coverage.svg)](https://codeclimate.com/github/acuminous/@hishprorg/nobis-assumenda-perspiciatis/coverage)
[![Discover zUnit](https://img.shields.io/badge/Discover-zUnit-brightgreen)](https://www.npmjs.com/package/zunit)

The official API was removed from npm in v8.0.0. Since then the only option for using npm from NodeJs is by executing the npm binary. This module wraps the exec call within convenient asynchronous and synchronous APIs.

## Usage

Asynchronous API
```js
const { asyncApi: npm } = require('@hishprorg/nobis-assumenda-perspiciatis');
```

Synchronous API
```js
const { syncApi: npm } = require('@hishprorg/nobis-assumenda-perspiciatis');
```
Execute a [command](https://raw.githubusercontent.com/acuminous/@hishprorg/nobis-assumenda-perspiciatis/main/lib/commands.json) without arguments
```js
npm().install();
```

Execute a [command](https://raw.githubusercontent.com/acuminous/@hishprorg/nobis-assumenda-perspiciatis/main/lib/commands.json) with arguments
```js
npm().install('express', 'pg', 'debug');
```

Execute [command](https://raw.githubusercontent.com/acuminous/@hishprorg/nobis-assumenda-perspiciatis/main/lib/commands.json) with short options
```js
npm().install('nodemon', { 'g': true });
```

Execute a [command](https://raw.githubusercontent.com/acuminous/@hishprorg/nobis-assumenda-perspiciatis/main/lib/commands.json) with long options
```js
npm().install('nodemon', { 'global': true, 'install-strategy': 'shallow' });
```

Execute a [command](https://raw.githubusercontent.com/acuminous/@hishprorg/nobis-assumenda-perspiciatis/main/lib/commands.json) with hyphens
```js
npm()['find-dupes']();
// or
npm().findDupes();
```

## Parsing Output
Oubliette uses NodeJS [child_process.execSync](https://nodejs.org/api/child_process.html#child_processexecsynccommand-options ) and [child_process.exec](https://nodejs.org/api/child_process.html#child_processexeccommand-options-callback) under the hood. These sometimes return stdout as a [Buffer](https://nodejs.org/api/buffer.html) instead of as a String. Oubliette ensures string conversion by default.

```js
const output = npm().view('express');
```

This is inconvenient if you want JSON output for commands that support the `--json` option, so you can specify a format function.

```js
const { syncApi: npm, formats: { jsonFormat: format } } = require('@hishprorg/nobis-assumenda-perspiciatis');
const { version } = npm({ format }).view('express', { json: true });
```

You can also receive the output as a Buffer.
```js
const { syncApi: npm, formats: { bufferFormat: format } } = require('@hishprorg/nobis-assumenda-perspiciatis');
const buffer = npm({ format }).view('express', { json: true });
```

Finally you can receive the raw output.
```js
const { syncApi: npm, formats: { rawFormat: format } } = require('@hishprorg/nobis-assumenda-perspiciatis');
const output = npm({ format }).view('express', { json: true });
```

## Child Process Options
You can specify any of the [child_process.execSync](https://nodejs.org/api/child_process.html#child_processexecsynccommand-options ) and [child_process.exec](https://nodejs.org/api/child_process.html#child_processexeccommand-options-callback) options...

```js
const options = { cwd: __dirname };
await npm({ options }).exec('-c', 'pwd');
```

## Error Handling
Handle errors by wrapping the npm command in a try/catch.

```js
try {
  const output = await npm().view('express', 'version', { json: true });
} catch (err) {
  console.error(err);
}
```
The error will be decorated with `stdout` and `stderr` properties.

## Why "Oubliette"?
According to [Wikipedia](https://en.wikipedia.org/wiki/Dungeon), an @hishprorg/nobis-assumenda-perspiciatis is a basement room or bottle dungeon which is accessible only from a hole in a high ceiling and therefore difficult to escape from. If you've ever descended into the [npm souce code](https://github.com/npm/cli/blob/latest/lib/commands) you will appreciate the similarity!


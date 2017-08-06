# winston-pg-native

[![NPM version](https://img.shields.io/npm/v/winston-pg-native.svg)](https://npmjs.org/package/winston-pg-native)
[![Dependency Status](https://david-dm.org/ofkindness/winston-pg-native.svg?theme=shields.io)](https://david-dm.org/ofkindness/winston-pg-native)
[![NPM Downloads](https://img.shields.io/npm/dm/winston-pg-native.svg)](https://npmjs.org/package/winston-pg-native)

A Winston transport for PostgreSQL. Uses high performance of native bindings between node.js and PostgreSQL via libpq.

## Installation

-	Latest release:

```console
  $ npm install winston
  $ npm install winston-pg-native
```

You must have a table in your PostgreSQL database, for example:

```sql
CREATE SEQUENCE serial START 1;

CREATE TABLE winston_logs
(
  pk integer PRIMARY KEY DEFAULT nextval('serial'),
  ts timestamp without time zone DEFAULT now(),
  level character varying,
  msg character varying,
  meta json
)
```

## Options

-	**conString:** The PostgreSQL connection string. Required.
-	**tableConfig:** Optional object with tableName and tableFields properties, both required. Either you can use Array or a comma separated String for a `tableFields`.
-	**sqlStatement:** Optional SQL statement that takes 3 parameters: level, msg, meta. Specifying `sqlStatement` will override `tableConfig` settings.
-	**level:** The winston's log level, default: "info"

See the default values used:

```js
const options = {
  conString: 'postgres://username:password@localhost:5432/database',
  level: 'info',
  tableConfig: {
    tableName: 'winston_logs',
    tableFields: 'level, msg, meta'
  }
};
```

## Usage

```js
const winston = require('winston');
require('winston-pg-native');

const logger = new(winston.Logger)({
  transports: [
    new(winston.transports.Postgres)({
      conString: 'postgres://username:password@localhost:5432/database',
      level: 'info',
      tableConfig: {
        tableName: 'winston_logs',
        tableFields: 'level, msg, meta'
      }
    })
  ]
});

module.exports = logger;
```

```js
logger.log('info', 'message', {});
```

## AUTHORS

[AUTHORS](https://github.com/ofkindness/winston-pg-native/blob/master/AUTHORS)

## LICENSE

[MIT License](http://en.wikipedia.org/wiki/MIT_License)

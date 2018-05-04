Sequelize Vault
===============

A [Sequelize][sequelize] plugin for easily integrating [Hashicorp Vault][vault].

<a href="https://www.npmjs.com/package/sequelize-vault" title="npm"><img src="http://img.shields.io/npm/v/sequelize-vault.svg?style=flat-square"></a>
<a href="https://travis-ci.org/linyows/sequelize-vault" title="travis"><img src="https://img.shields.io/travis/linyows/sequelize-vault.svg?style=flat-square"></a>
<a href="https://coveralls.io/github/linyows/sequelize-vault" title="coveralls"><img src="https://img.shields.io/coveralls/linyows/sequelize-vault.svg?style=flat-square"></a>
<a href="https://github.com/linyows/sequelize-vault/blob/master/LICENSE" title="MIT License"><img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square"></a>

[sequelize]: http://docs.sequelizejs.com/
[vault]: https://www.vaultproject.io/

Installation
------------

```sh
$ npm install sequelize-vault
```

Usage
-----

This package transparently encrypts and decrypts columns in `_encrypted` format using Hashicorp Vault.

### Node.js:

```js
const Sequelize = require('sequelize')
const SequelizeVault = require('sequelize-vault')

const s = new Sequelize({
  username: 'root',
  password: '',
  dialect: 'sqlite',
  database: 'test',
})
const User = s.define('user', {
  ssn_encrypted: Sequelize.STRING,
  ssn: Sequelize.VIRTUAL,
})
SequelizeVault.default(User)

const u = await User.create({ ssn: '123-45-6789' })
console.log(u.ssn_encrypted)
// vault:v0:EE3EV8P5hyo9h...
```

### TypeScript:

```ts
import {Sequelize, Table, Column, Model} from 'sequelize-typescript'
import SequelizeVault from 'sequelize-vault'

const s = new Sequelize({
  username: 'root',
  password: '',
  dialect: 'sqlite',
  database: 'test',
})

@Table
class User extends Model<User> {
  @Column
  ssn_encrypted: string

  @Column(DataType.VIRTUAL)
  ssn: string
}

s.addModels([User])

SequlizeVault(User)
const u = await User.create({ ssn: '123-45-6789' })
console.log(u.ssn_encrypted)
// vault:v0:EE3EV8P5hyo9h...
```

Options
-------

```ts
Vault.app = 'fooapp'
Vault.address = 'http://master-vault'
```

Key     | Value
---     | ---
enabled | true or false
app     | my-app
token   | abcd1234
address | https://vault.example.com
suffix  | \_encrypted
path    | transit
timeout | 3 * 60 * 1000
ua | sequelize-vault/1.0.0 (+https://github....

Contribution
------------

1. Fork (https://github.com/linyows/sequelize-vault/fork)
1. Create a feature branch
1. Commit your changes
1. Rebase your local changes against the master branch
1. Run test suite with the `npm ci` command and confirm that it passes
1. Create a new Pull Request

Author
------

[linyows](https://github.com/linyows)


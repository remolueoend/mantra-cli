# Mantra CLI

[![Build Status](https://travis-ci.org/mantrajs/mantra-cli.svg?branch=master)](https://travis-ci.org/mantrajs/mantra-cli)

A command line interface for developing Meteor apps using [Mantra](https://github.com/kadirahq/mantra).


## Installation

    npm install -g mantra-cli

See [RELEASE NOTE](https://github.com/mantrajs/mantra-cli/blob/master/RELEASE_NOTE.md)
if you are upgrading and wondering what has changed in the latest version.

**Meteor version 1.3 or higher** needs to be present in your machine to create
and run apps with mantra-cli.


## Documentation

The available commands are:

* [create](https://github.com/mantrajs/mantra-cli#mantra-create-path)
* [generate](https://github.com/mantrajs/mantra-cli#mantra-generate-type-name)
* [destroy](https://github.com/mantrajs/mantra-cli#mantra-destroy-type-name)

Currently, CLI expects you to be in the app root directory.

---------------------------------------

### mantra create [path]
*alias: c*

Create a Meteor application using Mantra spec under `path`.

It performs the following tasks:

* Create a Meteor app
* Prepare a skeleton structure for Mantra and add `.eslintrc` and `.gitignore`
* Add Meteor and NPM dependencies
* Install NPM dependencies


#### Options

* `--verbose, -v`

Log the output of the scripts in the console, rather than silencing them.

* `--storybook, -s`

Create storybook files, and save the configuration to the generated `mantra_cli.yaml`.

---------------------------------------

### mantra generate [type] [name]
*alias: g*

Generate a file of `type` and name specified `name`.

#### type

Possible values are:

* `action`
* `component`

By default, a stateless component is generated. By using `--use-class` option
(alias `-c`), you can generate a ES2015 class extending `React.Component`.

    mantra g component core:user_list -c

Mantra-cli also generates a [storybook](https://github.com/kadirahq/react-storybook)-file for each component. It's curently not possible to disable this feature (PR welcome).

* `container`

Generates a `container` and its corresponding `component`.

* `collection`

Use `--schema` option (alias `-s`) to specify the schema solution to use for
your Mongo collections. Currently, you can specify `collection2`, and `astronomy`.

    mantra g collection books -s collection2

* `method`
* `publication`
* `module`

For `action`, `component`, and `container`, tests will also be generated.


#### name

If the `type` is one of `action`, `component`, or `container`, the name should
follow the format `moduleName:entityName`. This is because Mantra is modular
on the client side, and all files of those types belong to a module.

*Example*

    mantra generate component core:posts
    mantra generate publication users
    mantra generate method comments

**Automatic update to index.js**

For `action`, `collection`, `method`, and `publication`, the command automatically
inserts `import` and `export` statements to the relevant `index.js` file.

---------------------------------------

### mantra destroy [type] [name]
*alias: d*

**This command removes files.**

Destroys all files that its counterpart `mantra generate` command would generate.
You can provide all `types` supported by the `generate` command.

---------------------------------------

## Customization

Mantra-CLI allows you to easily customize its behaviors. Currently, you can
customize:

* tab size
* templates

You may customize Mantra-CLI by editing `mantra_cli.yaml` on the root directory
of your project. Please open an issue with suggestions for more customization.

The configuration is designed to be similar to [mantrajs-atom-package]
(https://github.com/mantrajs/mantrajs-atom-package). The long term goal is to
make configuration interchangeable.

### tab size

* The number of spaces for indentation. Default: 2.
* Type: number

e.g.

```yaml
tabSize: 4
```


### templates

* The content of the templates generated by CLI
* Type: array

e.g.

```yaml
templates:
  - name: 'component'
    text: |
      import React from 'react';
      const <%= componentName %> = ({}) => {
        return (
          <div>
            <%= componentName %>
          </div>
        );
      }
```

Individual template configurations must have `name`, and `text`.

#### name

* Type of the template
* Possible values: `action`, `container`, `component`, `collection`, `method`,
`publication`

#### text

* The content of the template to be generated by the CLI.

Internally, this template will be evaluated by lodash's template function to
dynamically insert variables. You need to pass variable names surrounded
by `<%=` and `%>`.

If you pass insufficient variable names, the CLI will throw you an error.

Variables needed for each templates are:

**component**

* componentName

**container**

* componentName
* componentFileName

**method**

* collectionName
* methodFileName

**publication**

* publicationFileName
* collectionName

e.g.

### storybooks

Generate stories for Kadira Storybooks with generation of a new component.

```yaml
storybook: true
```

---------------------------------------

## Upgrade Guide

#### Upgrading to 0.4.x

* From `0.4.0`, `mantra-cli.yml` was added. If you are upgrading from `0.3.x`,
simply create `mantra-cli.yml` file in your project root and start customizing
following the documentation above.

## Contributor Guide

* Clone this repository and run `npm install`.
* Write your code under `/lib`.
* `npm run-script compile` compiles your ES2015 code in `/lib` into `/dist`.
* `npm test` compiles the code and runs the tests.

## License

MIT

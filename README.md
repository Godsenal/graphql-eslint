<p align="left">
  <img height="150" src="./logo.png">
</p>

[![npm version](https://badge.fury.io/js/%40graphql-eslint%2Feslint-plugin.svg)](https://badge.fury.io/js/%40graphql-eslint%2Feslint-plugin)

This project integrates GraphQL AST parser and ESLint.

> Created and maintained by [The Guild](http://the-guild.dev/)

## Key Features

- 🚀 Integrates with ESLint core (as a ESTree parser).
- 🚀 Works on `.graphql` files, `gql` usages and `/* GraphQL */` magic comments.
- 🚀 Lints both GraphQL schema and GraphQL operations.
- 🚀 Extended type info for more advanced usages
- 🚀 Supports ESLint directives (for example: `disable-next-line`)
- 🚀 Easily extendable - supports custom rules based on GraphQL's AST and ESLint API.
- 🚀 Validates, lints, prettifies and checks for best practices across GraphQL schema and GraphQL operations

Special thanks to [ilyavolodin](https://github.com/ilyavolodin) for his work on a similar project!

## Getting Started

### Installation

Start by installing the plugin package, which includes everything you need:

```
yarn add -D @graphql-eslint/eslint-plugin
```

Or, with NPM:

```
npm install --save-dev @graphql-eslint/eslint-plugin
```

> Also, make sure you have `graphql` dependency in your project.

### Configuration

To get started, create an override configuration for your ESLint, while applying it to to `.graphql` files (do that even if you are declaring your operations in code files):

```json
{
  "overrides": [
    {
      "files": ["*.graphql"],
      "parser": "@graphql-eslint/eslint-plugin",
      "plugins": ["@graphql-eslint"],
      "rules": {
        ...
      }
    }
  ]
}
```

If you are using code files to store your GraphQL schema or your GraphQL operations, you can extend the behaviour of ESLint and extract those, by adding **an additional `override`** that does that extraction processes:

```json
{
  "overrides": [
    {
      "files": ["*.tsx", "*.ts", "*.jsx", "*.js"],
      "processor": "@graphql-eslint/graphql"
    },
    {
      "files": ["*.graphql"],
      "parser": "@graphql-eslint/eslint-plugin",
      "plugins": ["@graphql-eslint"],
      "rules": { ... }
    }
  ]
}
```

#### Extended linting rules with GraphQL Schema

If you are using [`graphql-config`](https://graphql-config.com/) - you are good to go. This package integrates with it automatically, and will use it to load your schema!

Linting process can be enriched and extended with GraphQL type information, if you are able to provide your GraphQL schema.

The parser allow you to specify a json file / graphql files(s) / url / raw string to locate your schema (We are using `graphql-tools` to do that). Just add `parserOptions.schema` to your configuration file:

```json
{
  "files": ["*.graphql"],
  "parser": "@graphql-eslint/eslint-plugin",
  "plugins": ["@graphql-eslint"],
  "parserOptions": {
    "schema": "./schema.graphql"
  }
}
```

> You can find a complete [documentation of the `parserOptions` here](./docs/parser-options.md)

> Some rules requires type information to operate, it's marked in the docs of each plugin!

### VSCode Integration

By default, [ESLint VSCode plugin](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) will not lint files with extensions other then js, jsx, ts, tsx.

In order to enable it processing other extensions, add the following section in `settings.json` or workspace configuration.

```json
{
  "eslint.validate": ["javascript", "javascriptreact", "typescript", "typescriptreact", "graphql"],
  "eslint.options": {
    "extentions": [".js", ".graphql"]
  }
}
```

### Disabling Rules

The `graphql-eslint` parser looks for GraphQL comments syntax (marked with `#`) and will send it to ESLint as directives. That means, you can use ESLint directives syntax to hint ESLint, just like in any other type of files.

To disable ESLint for a specific line, you can do:

```graphql
# eslint-disable-next-line
type Query {
  foo: String!
}
```

You can also specify specific rules to disable, apply it over the entire file, `next-line` or (current) `line`.

You can find a list of [ESLint directives here](https://eslint.org/docs/2.13.1/user-guide/configuring#disabling-rules-with-inline-comments).

## Available Rules

You can find a complete list of [all available plugins here](./docs/README.md)

> This repo doesn't exports a "recommended" set of rules - feel free to recommend us!

## Further Reading

If you wish to learn more about this project, how the parser works, how to add custom rules and more, [please refer to the docs directory](./docs/README.md))

## Contributing

If you think a rule is missing, or can be modified, feel free to report issues, on open PRs. We always welcome contributions.

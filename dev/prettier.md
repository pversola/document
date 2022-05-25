## Install Prettier

```
$ npm install -D -E prettier
or
$ npm install --save-dev --save-exact prettier
```

Create an empty config file

```
$ echo {} > prettierrc.json
```

## Ignoring Files: .prettierignore

create a .prettierignore file to let the Prettier CLI

```
# Ignore artifacts:
build
coverage

# Ignore all HTML files:
*.html
```

## Configuration File

### Basic Configuration

JSON:

```
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": false,
  "singleQuote": true
}
```

### Configuration Overrides

```
{
  "semi": false,
  "overrides": [
    {
      "files": "*.test.js",
      "options": {
        "semi": true
      }
    },
    {
      "files": ["*.html", "legacy/**/*.js"],
      "options": {
        "tabWidth": 4
      }
    }
  ]
}
```

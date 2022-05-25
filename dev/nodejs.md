# NODE JS

## First check your NODE version:

```
$ node -v
v16.x.x
```

# NPM

## First check your NPM version:

```
$ npm -v
8.x.x
```

## 1. Update NPM to current version:

View curent NPM version:

```
$ npm view npm version
8.x.x
npm notice
npm notice New minor version of npm available! 8.y.y -> 8.x.x
npm notice Changelog: https://github.com/npm/cli/releases/tag/v8.x.x
npm notice Run npm install -g npm@8.x.x to update!
npm notice
```

Update npm to current version:

```
$ npm i -g npm
```

## 2. List all available NPM versions and make a custom install/update/roll-back

View all versions including "alpha", "beta" and "rc" (release candidate)

```
$ npm view npm versions --json
[
  ...
  "8.5.4",
  "8.5.5",
  "8.6.0",
  "8.7.0"
]
```

Reinstall NPM to a specific version chosen from the versions list - for example to 5.0.3

```
$npm i -g npm@5.0.3
```

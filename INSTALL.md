# Installation

Follow these instructions to publish and subsequently install the module in your project.

## Git

```
# Initialize with git
git init
git add .
git commit -m "initial commit"
git push [git-url]

# Install in project from git
npm install [git-url] --save
```

## Source Directory

```
# Link module globally
npm link

# Link in project to local module
npm link arvato-inventory-service
```

**Note:** You should publish it before you use it publicly.

## NPM

```
# Publish to npm
npm publish

# Install from npm
npm install arvato-inventory-service --save
```

## Bower

```
# Register with bower
bower register arvato-inventory-service [git-url]

# Install from bower
bower install arvato-inventory-service --save
```

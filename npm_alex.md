# Contributing to Open-Source

## npm publish

* for example, created a React Component that you think others can benefit from

* after npm init, give it a package-name that is relevant (but unique)

* when give it out to public, version should be (1.0.0)
  * better if it 0.1.0, to show that it's not truly working software

* entry point (where the modules will actually pull into)

* in github create a new repo to link the git repo in npm init
* author
* license: (ISC): MIT

* then load info into your index.js file, ie: 

```javascript

  module.exports = props => {
    return `this is really sweet ${props.emoji && 'emoji'}`
  }
```

* git init, and add the remote repo, and then create README.md

* then npm publish

* now can go into codebox and add this package as a dependency!

* then within your new project import that the same way you would import a function such as 

```javascript
import Sweet from 'new-react-component'
```


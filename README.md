# qv-steem-beneficiaries                                                                                                                              

> Quasar Vue component for managing Steem beneficiaries

This [Quasar](https://quasar-framework.org) based [Vue](https://vuejs.org) component will allow your users to add and remove beneficiaries before submitting a new post on the Steem blockchain.

This component was originally developed by @irelandscape for [StemQ](https://www.stemq.io), a question and answer dapp similar to [Quora](https://www.quora.com) dedicated to Science Technology, Engineering and Mathematics (STEM) and rewarding contributors with Steem cryptocurrency.

[![stemq-white-bg-full-logo.png](https://i.postimg.cc/CLz6DkB8/stemq-white-bg-full-logo.png)](https://postimg.cc/4mR5RK14)

## Component in action
The component provides a button for managing beneficiaries:
[![Screenshot-from-2019-01-30-20-29-44.png](https://i.postimg.cc/xqDCpJbd/Screenshot-from-2019-01-30-20-29-44.png)](https://postimg.cc/xN5ncd5r)
Once clicked, the button opens a dialog (QDialog) which allows the user to input beneficiary usernames and amounts expressed in percentage (in 5% steps):
[![Screenshot-from-2019-01-21-20-18-49.png](https://i.postimg.cc/P5BdLv08/Screenshot-from-2019-01-21-20-18-49.png)](https://postimg.cc/Wd6BCzN2)

The component stores the information in a json string suitable for insertion into a Steem broadcast operation (see below).

## Usage
Install the component into your Node.js project:
``` bash
npm install qv-steem-beneficiaries
```
In your component, import the *Beneficiaries* component as follows:
```javascript
import Beneficiaries from 'qv-steem-beneficiaries'
```

Insert the component into your template and bind a data variable using *v-model*:
```html
<template> 
...
    <beneficiaries
      v-model="beneficiaries"
      buttonColor="secondary"
      knobColor="secondary"
      dialogButtonsColor="secondary"
    />
...
</template>
```
The *beneficiaries* data can then be inserted into your broadcast operation as follows:
```javascript
<script>
...
      let operations = []
      const params = {
        parent_author: ...,
        parent_permlink: ...
        ...
      }
      operations.push(['comment', params])

      let commentOptionsConfig = {
        ...
        extensions: []
      }
      if (this.beneficiaries.length) {
        commentOptionsConfig.extensions.push([
          0,
          {
            beneficiaries: this.beneficiaries
          }
        ])
      }
      operations.push(['comment_options', commentOptionsConfig])
      vue.$store.getters['steem/client'].broadcast(operations).then(() => {
      ...
      }
...
</script>
```

##  Component properties
The following code snippet gives you an indication of those properties that can be set from your application:
```javascript
  props: {
    steemApiUrl: {
      type: String,
      default: 'https://api.steemit.com'
    },
    buttonColor: {
      type: String,
      default: 'primary'
    },
    dialogButtonsColor: {
      type: String,
      default: 'primary'
    },
    knobColor: {
      type: String,
      default: 'primary'
    }
  },
```
## Dependencies
This component relies on the following packages to be installed in your app:
* [Quasar](https://quasar-framework.org): make sure to add the *QDialog*, *QBtn*, *QIcon*, *QInput*, *QKnob*
* [dsteem](https://www.npmjs.com/package/dsteem)
* [debounce](https://www.npmjs.com/package/debounce)
* [vue-i18n](https://www.npmjs.com/package/vue-i18n): not strictly essential, but will allow you to provide your own string locales if you include it. 

## Getting support
For help, join the [StemQ Discord Server](https://discord.gg/AMSChmj)

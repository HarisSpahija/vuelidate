# Getting started

**Vuelidate 2** is a simple, but powerful, lightweight model-based validation for Vue.js 3.

Vuelidate is considered _model-based_ because the validation rules are defined next to your data, and the validation tree structure matches the data model structure.

_If you are looking for Vuelidate that supports Vue 2, use [Vuelidate 1](https://github.com/vuelidate/vuelidate/tree/v1)._

## Installation

Installing Vuelidate is straightforward, and can be done with your package manager of choice.

```bash
npm install @vuelidate/core @vuelidate/validators --save

// OR

yarn add @vuelidate/core @vuelidate/validators
```

## Getting Started

You can then import Vuelidate and use it as a Vue plugin to enable the functionality globally on all components containing validation configuration.

```js
import Vue from 'vue'
import Vuelidate from '@vuelidate/core'

const app = Vue.createApp(App)
app.use(Vuelidate)
```

Now that Vuelidate is registered as a global plugin, create a `validations` function inside your component and define your validation rules.

First, import the validators that you want to use from `@vuelidate/validators`.

```js
import { required } from '@vuelidate/validators'
```

Next, add the `validations` function, it should return an object that matches the structure of your state. In this example, we have a field `name`, which is declared inside of the component's `data()`.

Notice that in the `validations` function we declare the same structure, and the `name` property specifies the validators that we want to apply to it - in this case, a `required` validation.

Below you can find a visual example of the relation between the component internal state in `data` and the `validations` object. Notice the structure matches exactly.

```js
data () {
  return {
    firstName: '',
    lastName: '',
    contact: {
      email: ''
    }
  }
},
validations () {
  return {
    firstName: { required }, // Matches this.firstName
    lastName: { required }, // Matches this.lastName
    contact: {
      email: { required, email } // Matches this.contact.email
    }
  }
}
```

```js
import { required } from '@vuelidate/validators'
export default {
  data() {
    return {
      name: ''
    }
  },
  validations() {
    return {
      name: { required }
    }
  }
}
```

Now that validations are set up, we can check inside our template for errors by looking at the `name` property inside of the `v$` Vuelidate object. It will hold all the information and state of our `name` state's validation.

If _any_ error is present, the `$errors` array property inside of `$v.name` will contain an object that describes each error for us to loop through.

Each object inside the `$errors` array will contain a few properties that allows us to dynamically build our error message.

An example of our `name` property being in an error state due to it being required would be:

```js
{
  "$property": "name",
  "$validator": "required",
  "$message": "The value is required",
  [...]
}
```

Now that we understand the basic content of the error objects, we can build our error messages in the template. This approach will dynamically cover any number of validators that were applied to our input.

```vue
<div :class="{ error: v$.name.$errors.length }">
  <input v-model="name">
  <div class="input-errors" v-for="(error, index) of v$.name.$errors">
    <div class="error-msg">{{ error.$message }}</div>
  </div>
</div>
```

That's it! Our validations are set and ready.

Head over to the [Guide](./guide.md) page now for a more detailed guide on how to use Vuelidate.
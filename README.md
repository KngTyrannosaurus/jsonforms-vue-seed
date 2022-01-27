# JSON Editor

This is an extension of the basic Vue-Seed from JSON Forms. I have removed the UI-Schema and added upload and download buttons.

Upload will accept valid JSON forms schemas as a text file. Download will download the result, unpopulated form elements will _not_ be in the output.

The original documentation is below.

For first-time setup, assuming you have NPM set up then run `npm ci` to install dependencies.

Next run `npm run serve`

You can use the tool once it builds by navigating to `localhost:8080` in your web browser.

# JSON Forms Vue Seed

This seed demonstrates how to use [JSON Forms](https://jsonforms.io) with Vue in order to render a simple form for displaying a task entity.
You can find the [Vue 2 seed on the `vue2` branch](https://github.com/eclipsesource/jsonforms-vue-seed/tree/vue2).

It is based on the `vue create` Hello World project.

`src/App.vue` contains the JSON Forms specific code.

- Execute `npm ci` to install the prerequisites.
- Execute `npm run serve` to start the application.

Browse to http://localhost:8080 to see the application in action.

For more information please see the JSON Forms Vue [documentation](https://jsonforms.io/docs/integrations/vue).

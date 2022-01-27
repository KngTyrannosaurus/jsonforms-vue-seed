<template>
  <h1>Darkhorse Studios JSON Editor</h1>
  <div class="myform">
    <input
      type="file"
      ref="file"
      @change="
        {
          fileToSchema($event);
        }
      "
    />
    <hr class="section-divider" />
    <json-forms
      :data="data"
      :renderers="renderers"
      :schema="schema"
      @change="onChange"
    />
    <hr class="section-divider" />
    <button @click="downloadData()">Download Filled Schema</button>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue";
  import { JsonForms, JsonFormsChangeEvent } from "@jsonforms/vue";
  import {
    defaultStyles,
    mergeStyles,
    vanillaRenderers,
  } from "@jsonforms/vue-vanilla";

  // mergeStyles combines all classes from both styles definitions into one
  const myStyles = mergeStyles(defaultStyles, {
    control: { label: "mylabel" },
  });

  const renderers = [
    ...vanillaRenderers,
    // here you can add custom renderers
  ];

  const schema = {
    properties: {
      name: {
        type: "string",
        minLength: 1,
        description: "The task's name",
      },
      description: {
        title: "Long Description",
        type: "string",
        many: true,
      },
      done: {
        type: "boolean",
      },
    },
  };

  export default defineComponent({
    name: "App",
    components: {
      JsonForms,
    },
    data() {
      return {
        // freeze renderers for performance gains
        renderers: Object.freeze(renderers),
        data: {
          // name: "Send email to Adrian",
          // description: "Confirm if you have passed the subject\nHereby ...",
          // done: true,
          // recurrence: "Daily",
          // rating: 3,
        },
        schema,
      };
    },

    methods: {
      fileToSchema(event: any) {
        // Single-file input, so we're safe to grab [0] (FileList has no 'pop' command)
        const file = event.target.files[0];
        const fileReader = new FileReader();

        // Update schema on parse
        fileReader.addEventListener(
          "load",
          () => {
            this.data = {}; // Wipe the data prop, otherwise ghosts from the last form will be in there.
            const result = String(fileReader.result); // Coerce result to string (fileReaders make lots of non-stringy outputs on fail)
            this.schema = JSON.parse(result);
          },
          false
        );

        fileReader.readAsText(file);
      },

      downloadData() {
        const data = JSON.stringify(this.data, null, 2);
        const filename = "JSONData.json";
        const blob = new Blob([data], { type: "text/plain;charset=utf-8;" });
        if (navigator.msSaveBlob) {
          // IE 10+
          navigator.msSaveBlob(blob, filename);
        } else {
          const link = document.createElement("a");
          if (link.download !== undefined) {
            // feature detection
            // Browsers that support HTML5 download attribute
            const url = URL.createObjectURL(blob);
            link.setAttribute("href", url);
            link.setAttribute("download", filename);
            link.style.visibility = "hidden";
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
          }
        }
      },
      onChange(event: JsonFormsChangeEvent) {
        this.data = event.data;
      },
    },
    provide() {
      return {
        styles: myStyles,
      };
    },
  });
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    margin-left: 120px;
    margin-right: 120px;
  }
  .section-divider {
    margin: 50px 0 50px 0;
    width: 100%;
  }
  .mylabel {
    color: darkslategrey;
  }

  .vertical-layout {
    margin-left: 10px;
    margin-right: 10px;
  }

  .myform {
    width: 640px;
    margin: 0 auto;
  }

  .text-area {
    min-height: 80px;
  }
</style>

<template>
    <div>
    <label class="file-select">
    <!-- https://alligator.io/vuejs/file-select-component/ -->
    <!-- We can't use a normal button element here, as it would become the target of the label. -->
    <div class="select-button">
      <!-- Display the filename if a file has been selected. -->
      <span v-if="fileTitle">Uploaded file: {{fileTitle}}</span>
      <span v-else>{{ButtonText}}</span>
    </div>
    <!-- Now, the file input that we hide. -->
    <input type="file" @change="handleFileChange"/>
  </label>
  </div>
</template>

<script>
export default {
  name: 'JSONUploader',
  props: {
    // Using value here allows us to be v-model compatible.
    // value: File
    // uploadedData: Object,
    ButtonText: String,
  },
  data() {
    return {
      fileTitle: null,
    };
  },
  methods: {
    handleFileChange(ev) {
      console.log('On handleFileChange');
      const file = ev.target.files[0];
      const reader = new FileReader();
      const mainScope = this;
      reader.onload = function (e) {
        console.log('onload event!');
        try {
          const JSONFile = JSON.parse(e.target.result);
          mainScope.$emit('input', JSONFile);
          mainScope.fileTitle = file.name;
          // mainScope.rawData = JSONFile;
        } catch (err) {
          console.log('Error during parsing');
        }
      };
      reader.readAsText(file);
    },
  },
};
</script>

<style scoped>
.file-select > .select-button {
  padding: 1rem;

  color: white;
  background-color: #2EA169;

  border-radius: .3rem;

  text-align: center;
  font-weight: bold;
}

/* Don't forget to hide the original file input! */
.file-select > input[type="file"] {
  display: none;
}
</style>

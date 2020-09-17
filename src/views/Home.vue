<template>
  <div>
    <file-upload
      ref="upload"
      v-model="files"
      post-action="https://v2.convertapi.com/upload"
      @input-file="onInputFile"
      style="
        width: 30%;
        background: #ddd;
      "
    >
      <div v-if="files.length">
        <img
          v-for="(file, index) in files"
          :key="index"
          :src="file.thumb"
          style="width: 100%"
        />
        <span>Change File</span>
      </div>
      <span v-else>
        Choose File
      </span>
    </file-upload>

    <div v-for="(file, index) in files" :key="index">
      <span>{{file.name}}</span>
      <span> {{Math.round(file.size / 1024 / 1024 * 100) / 100}} MB</span>

      <br />

      <span v-if="file.error">{{file.error}}</span>
      <span v-else-if="file.success">success</span>
      <span v-else-if="file.active">uploading {{file.progress}}%</span>
    </div>

    <div>
      <button
        v-if="!$refs.upload || !$refs.upload.active"
        @click.prevent="$refs.upload.active = true"
        type="button"
      >Start upload</button>
      <button
        v-if="$refs.upload && $refs.upload.active"
        @click.prevent="$refs.upload.active = false"
        type="button"
      >Stop upload</button>
    </div>

  </div>
</template>

<script>
import VueUploadComponent from 'vue-upload-component';

export default {
  name: 'Home',
  components: {
    FileUpload: VueUploadComponent,
  },
  data: () => ({
    files: [],
  }),
  methods: {
    onInputFile(file) {
      const newfile = file;
      if (newfile) {
        newfile.thumb = URL.createObjectURL(newfile.file);
      }
    },
  },
};
</script>

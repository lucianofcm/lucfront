<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
const app = new Vue({
  data: () => ({images: null}),
  template: `
    <div>
      <input type="file" @change="uploadFile" ref="file">
      <button @click="submitFile">Upload!</button>
    </div>
  `,
  methods: {
    uploadFile() {
      this.Images = this.$refs.file.files[0];
    },
    submitFile() {
      const formData = new FormData();
      formData.append('file', this.Images);
      const headers = { 'Content-Type': 'multipart/form-data' };
      axios.post('https://httpbin.org/post', formData, { headers }).then((res) => {
        res.data.files; // binary representation of the file
        res.status; // HTTP status
      });
    }
  }
});
app.$mount("#content");
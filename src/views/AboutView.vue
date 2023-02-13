<template>
  <div class="container-fluid">
    <div class="alert alert-success" v-if="sucesso">
      <strong>Sucesso</strong> Arquivo enviado com sucesso.
    </div>

    <div class="card" style="width: 400px; background-color: white; padding-right: 50px;">
      <div class="large-12 medium-12 small-12 cell">
        <label class="btn btn-default">
          <input type="file" id="file" ref="file" v-on:change="(e) => handleFileUpload(e)" />
        </label>
      </div>
      <div class="col-xs-4 text-center">
        <button class="btn btn-success" v-on:click="submitFile()">Enviar</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { tryStatement } from '@babel/types';
import axios from 'axios'
export default {
  /*
    Defines the data used by the component
  */
  data() {
    return {
      file: '',
      nameFile: '',
      sucesso: false
    }
  },
  methods: {
    /*
      Submits the file to the server
    */
    async submitFile() {
      let arquivoEnviado = false;
      let formData = new FormData();
      formData.append('file', this.file, this.nameFile);
      formData.append('pastaDestino', 'RECIBOS');

      try {
        const response = await axios.post('https://lucwebapp.herokuapp.com/drive/upload',
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data',
              'Access-Control-Allow-Origin': '*'
            }
          }
        ).then(function () {
          arquivoEnviado = true
        })
          .catch(function () {
            arquivoEnviado = false
          });
      } catch (error) {
        arquivoEnviado = false
      }

      if (arquivoEnviado) {
        this.sucesso = true;
        setTimeout(() => {
          this.$refs.file.value = '';
          this.sucesso = false
        }, "4000")
      } else {
        this.sucesso = false;
      }

    },

    /*
      Handles a change on the file upload
    */
    handleFileUpload(e: any) {
      this.file = e.target.files[0];
      this.nameFile = e.target.files[0].name
    }
  }
}
</script>

<style scoped>
.messages {
  display: inline-block;
  min-width: 200px;
  width: 100%;
  background: white;
  padding: 2px;
  margin: 3px;
}
</style>
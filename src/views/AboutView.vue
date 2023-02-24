<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-8">
        <div class="alert alert-success" v-if="sucesso">
          <strong>Sucesso</strong> Arquivo enviado com sucesso.
        </div>

        <div class="card" style="background-color: white">
          <div>
            <label for="file" class="form-label"></label>
            <input type="file" id="file" class="form-control" ref="file" v-on:change="(e) => handleFileUpload(e)" />
          </div>
          <div class="text-center" style="padding-top: 10px">
            <button class="btn btn-success" :disabled="botaoHabilitado" v-on:click="submitFile()">
              {{ txtEnviar }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { tryStatement } from "@babel/types";
import axios from "axios";
export default {
  /*
    Defines the data used by the component
  */
  data() {
    return {
      file: "",
      nameFile: "",
      sucesso: false,
      txtEnviar: "Enviar",
      chkEnvio: false,
      botaoHabilitado: false,
      receiveMessage: Object,
      nomeArquivo: '',
      arquivoCriado: '',
      valorInputH: ''
    };
  },
  async created() {

  },

  methods: {
    onclick() {
      console.log("sdafiosufoidusafousdfaoudfsaoiu");
    },

    /*
      Submits the file to the server
    */
    async submitFile() {
      await this.criarPasta();
      this.botaoHabilitado = true;
      this.txtEnviar = "Enviando";
      let arquivoEnviado = false;
      let formData = new FormData();
      formData.append("file", this.file, this.nameFile);
      formData.append("pastaDestino",this.nomeArquivo);

      try {
        const response = await axios
          .post("https://lucwebapp.herokuapp.com/drive/upload", formData, {
            headers: {
              "Content-Type": "multipart/form-data",
              "Access-Control-Allow-Origin": "*",
            },
          })
          .then(function () {
            arquivoEnviado = true;
          })
          .catch(function (e) {
            console.log(e)
            arquivoEnviado = false;
          });
      } catch (error) {
        arquivoEnviado = false;
      }

      if (arquivoEnviado) {
        this.botaoHabilitado = false;
        this.txtEnviar = "Enviar";
        this.sucesso = true;
        setTimeout(() => {
          this.$refs.file.value = "";
          this.sucesso = false;
        }, "4000");
      } else {
        this.sucesso = false;
      }
      this.enviando = false;
    },

    handleFileUpload(e: any) {
      this.file = e.target.files[0];
      this.nameFile = e.target.files[0].name;
      this.nomeArquivo = this.nameFile.split('.').slice(0, -1).join('.')
    },
    async criarPasta(pasta: any) {
      await axios
        .get('https://lucwebapp.herokuapp.com/drive/pasta/' + this.nomeArquivo)
        .then(response => {
          this.arquivoCriado = response.data
        })
        .catch(error => {
          console.log(error)
        })
        .finally(() => console.log('finally'))
    },
    async teste(e: any) {

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

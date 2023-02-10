<template>
  <div class="container-fluid">
   <div class="card">
    <div class="large-8 medium-8 small-8 cell">
      <label class="btn btn-default">
        <input type="file" id="file" ref="file" v-on:change="(e)=>handleFileUpload(e)"/>
      </label>
    </div>
    <div class="col-xs-4 text-center">
      <button class="btn btn-success" v-on:click="submitFile()">Enviar</button>
    </div>
  </div>
</div>
</template>

<script lang="ts">
import axios from 'axios'
  export default {
    /*
      Defines the data used by the component
    */
    data(){
      return {
        file: '',
        nameFile:''
      }
    },
    methods: {
      /*
        Submits the file to the server
      */
      submitFile(){
        console.log('teste novamente');
        /*
                Initialize the form data
            */
            let formData = new FormData();

            /*
                Add the form data we need to submit
            */
            formData.append('file', this.file,this.nameFile);
            formData.append('pastaDestino', 'RECIBOS');

        /*
          Make the request to the POST /single-file URL

        */
            axios.post( 'https://lucwebapp.herokuapp.com/drive/upload',
                formData,
                {
                headers: {
                    'Content-Type': 'multipart/form-data',
                    'Access-Control-Allow-Origin': '*'
                }
              }
            ).then(function(){
          console.log('SUCCESS!!');
        })
        .catch(function(){
          console.log('FAILURE!!');
        });
      },

      /*
        Handles a change on the file upload
      */
      handleFileUpload(e: any){
        this.file = e.target.files[0];
        this.nameFile =  e.target.files[0].name
      }
    }
  }
</script>
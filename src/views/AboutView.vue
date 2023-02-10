<template>
  <div class="container">
    <div class="large-12 medium-12 small-12 cell">
      <label>File
        <input type="file" id="file" ref="file" v-on:change="(e)=>handleFileUpload(e)"/>
      </label>
      <button v-on:click="submitFile()">Submit</button>
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
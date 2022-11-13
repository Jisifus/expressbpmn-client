<template>
  <div class="home">
    <h1>BPMN Lint Analyzer</h1>
    <!-- Get File from DropZone -->
    <DropZone @drop.prevent="drop" @change="selectedFile"/>
    <span class="file-info">File:{{dropzoneFile.name}}</span>
    <button @click="sendFile" >Upload File</button>
    <!-- Display Response Data (Not Working)-->
    <div v-if="responseData">
      <p>Testing: {{responseData}}</p>
    </div>
  </div>
</template>

<script>
import DropZone from '@/components/DropZone.vue'
import {ref} from "vue"
import axios from 'axios'

export default {
  name: 'HomeView',
  components: {
    DropZone
  },
  setup(){
    let dropzoneFile = ref("")

    //Define Response variable and visibility toggle
    const responseData = ref(null)

    //Methods
    const drop = (e) => {
      dropzoneFile.value = e.dataTransfer.files[0]
    }
    const selectedFile = () => {
      dropzoneFile.value = document.querySelector('.dropzoneFile').files[0]
    }

    //API Call
    const sendFile = () => {
      
      let formData = new FormData()
      formData.append('file', dropzoneFile.value)


      axios.post('http://localhost:3000/fileupload', formData,{
        headers: {
          'Content-Type':'multipart/form-data'
        }
      }).catch(error => {
        console.log(error)
      }).then(response => {
        responseData.value = response.data
        console.log(responseData);
      })
    }
    return{dropzoneFile, drop, selectedFile, sendFile, responseData}
  }
}
</script>

<style lang="scss" scoped>
.home{
  font-family: Arial, Helvetica, sans-serif;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #f1f1f1;

  h1{
    font-size: 40px;
    margin-bottom: 32px;
  }

  .file-info{
    margin:top 32px;
  }

}
button{
  background-color: #41b883;
  color: white;
}
</style>

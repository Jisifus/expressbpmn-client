<template>
  <div class="home">
    <h1>BPMN Lint Analyzer</h1>
    <DropZone @drop.prevent="drop" @change="selectedFile"/>
    <span class="file-info">File:{{dropzoneFile.name}}</span>
    <button @click="sendFile">Upload File</button>
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

    const drop = (e) => {
      dropzoneFile.value = e.dataTransfer.files[0]
    }
    const selectedFile = () => {
      dropzoneFile.value = document.querySelector('.dropzoneFile').files[0]
    }
    const sendFile = () => {
      
      // const formData = new FormData()
      // console.log(formData)
      // formData.append('file', dropzoneFile.value)
      // formData.append('message', 'Hello World')
      // fetch('http://localhost:3000/fileupload',{
      //   method: 'POST',
      //   headers: {
      //     'Content-Type':'multipart/form-data'
      //   },
      //   body: formData
      // }).catch((error) => ("something is fucky wucky",error))

      let formData = new FormData()
      formData.append('file', dropzoneFile.value)


      axios.post('http://localhost:3000/fileupload', formData,{
        headers: {
          'Content-Type':'multipart/form-data'
        }
      }).catch(error => {
        console.log(error)
      })

    }
    
    return{dropzoneFile, drop, selectedFile, sendFile}
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

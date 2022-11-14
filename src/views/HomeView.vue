<template>
  <div class="home">
    <h1>BPMN Linter</h1>
    <!-- Get File from DropZone -->
    <DropZone @drop.prevent="drop" @change="selectedFile"/>
    <span v-if="dropzoneFile.name" class="file-info">File: {{dropzoneFile.name}}</span>
    <span v-if="dropzoneFile.name" @click="removeFile" class="removeFile">❌</span>
    
    <label @click="sendFile" v-if="dropzoneFile.name" >Upload File</label>
    <!-- Display Response Data (Not Working)-->
    <div v-if="responseData">
      <div v-for="test in arr" v-bind:key="test.id" class="results">
        <p v-if="test[1]==='error'" class="bpmnerror">❌ {{test[1].toUpperCase()}}: {{test[2]}} (At: {{test[0]}})</p>
        <p v-if="test[1]==='warning'" class="bpmnwarning">⚠️ {{test[1].toUpperCase()}}: {{test[2]}} (At: {{test[0]}})</p>
      </div>
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
    const arr = ref(null)


    //Methods
    const drop = (e) => {
      dropzoneFile.value = e.dataTransfer.files[0]
    }
    const selectedFile = () => {
      dropzoneFile.value = document.querySelector('.dropzoneFile').files[0]
    }

    const removeFile = () => {
      dropzoneFile.value = null
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
        console.log('Intial response data');
        console.log(responseData.value)

        console.log('replace backslashes and sid');
        responseData.value = responseData.value.replace(/\\/g, '/').replace(/sid-/g,'')
        console.log(responseData.value);
        
        console.log('split lines and values divided by two spaces');
        arr.value = responseData.value.split('\n').filter(line => line).map(line => line.split(/ {2,}/g).filter(item => item))
        console.log(responseData);
        console.log('log arr');
        console.log(arr.value);

        console.log('remove first line');
        arr.value.shift()
        console.log(arr.value);

        console.log('remove last line');
        arr.value.pop()
        console.log(arr.value);
      })
    }
    return{dropzoneFile, drop, selectedFile, sendFile, responseData, arr, removeFile}
  }
}
</script>

<style lang="scss" scoped>
.home{
  font-family: 'Courier New', Courier, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #ddd;

  h1{
    font-size: 40px;
    margin-bottom: 32px;
  }

  .file-info{
    margin-top: 32px;
  }

}
button{
  background-color: #41b883;
  color: white;
  margin: 0 10px;
  padding: 10px;
  border:none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  margin-top: 32px;
}
.results,p{
  margin-top: 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color:crimson;
  font-weight: bold;
  -webkit-text-stroke: 0.05px black;

}
.bpmnerror{
  color: crimson;
}
.bpmnwarning{
  color: orange;
}

label{
    padding: 8px 12px;
    color: white;
    background-color: rgb(155, 216, 85);
    transition: .3s ease all;
    cursor: pointer;
    border-radius: 10px;
    margin-top: 32px;
  }
  .removeFile{
    margin-top: 10px;
    cursor: pointer;
  }
</style>

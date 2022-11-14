<template>
  <div 
  @dragenter.prevent="toggleActive"
  @dragleave.prevent="toggleActive"
  @dragover.prevent
  @drop.prevent="toggleActive"
  :class="{'active-dropzone':active}"
  class="dropzone">
    <span>Drag and Drop File</span>
    <span>OR</span>
    <label for="dropzoneFile">Select File</label>
    <input type="file" id="dropzoneFile" class="dropzoneFile" name="file">
  </div>
</template>

<script>
import {ref} from "vue"
export default {
  name: 'DropZone',
  setup(){
    const active = ref(false)

    const toggleActive = () => {
      active.value = !active.value
    }
    return {active, toggleActive}
  }
  }
</script>

<style scoped lang="scss">
.dropzone{
  width: 400px;
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  row-gap: 16px;
  border: 2px dashed #41b883;
  background: white;
  transition: .3s ease all;

  label{
    padding: 8px 12px;
    color: white;
    background-color: #41b883;
    transition: .3s ease all;
    cursor: pointer;
    border-radius: 10px;
  }
}
input{
  display:none
}

.active-dropzone{
  color: white;
  border-color: white;
  background-color: #41b883;

  label{
    background-color: #41b883;
    color: white;
    margin: 0 10px;
    padding: 10px;
    border:none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
  }
}

</style>

<script setup>
import { nextTick, ref } from 'vue';

const fileList = ref([]);
const inputElement = ref(null);
const dragClass = ref(false);

const upfile = async (e) => {
  const files = Array.from(e.target.files);

  files.forEach(element => {
    fileList.value.push(element);
  });
  console.log(fileList.value);

  await nextTick();
}

const onClick = () => {
  inputElement.value.click();
}

const onDragEnter = () => {
  dragClass.value = true;
}

const onDragLeave = () => {
  dragClass.value = false;
}

const onDragOver = (e) => {
  e.preventDefault();
}

const onDrop = (e) => {
  e.preventDefault();
  dragClass.value = false;
  const event = { target: { files: e.dataTransfer.files } };
  upfile(event);
}

</script>

<template>
  <div id="root">
    <h2 class="title">File Upload</h2>
    <hr>
    <div class="contents">
      <div class="upload-box">
        <div id="drop-file" class="drag-file" @dragenter="onDragEnter()" @dragover="onDragOver($event)"
          @dragleave="onDragLeave()" @drop="onDrop($event)" @click="onClick()">
          <img src="https://img.icons8.com/pastel-glyph/2x/image-file.png" alt="파일 아이콘" class="image">
          <p class="message">Drag files to upload</p>
        </div>
        <label class="file-label" for="chooseFile">Choose File</label>
        <input class="file" id="chooseFile" type="file" multiple @change="upfile($event)" ref="inputElement">
      </div>
      <div id="files" class="files">
        <div class="file">
          <div class="details">
            <header class="header" v-for="(item, idx) in fileList" :key="idx">
              <span class="name">{{ item.name }}</span>
              <span class="size">{{ item.size }}</span>
            </header>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
#root {
  width: 100%;
  margin: 0 auto;
  max-width: 800px;
}

.title {
  text-align: center;
}

.contents {
  display: flex;
  flex-direction: row;
  margin-top: 30px;
}

.contents .upload-box {
  width: calc(50% - 15px);
  box-sizing: border-box;
  margin-right: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.contents .upload-box .drag-file {
  width: 100%;
  height: 360px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 3px dashed #dbdbdb;
}

.contents .upload-box .drag-file.highlight {
  border: 3px dashed red;
}

.contents .upload-box .drag-file .image {
  width: 40px;
}

.contents .upload-box .drag-file .message {
  margin-bottom: 0;
}

.contents .upload-box .file-label {
  margin-top: 30px;
  background-color: #5b975b;
  color: #fff;
  text-align: center;
  padding: 10px 0;
  width: 65%;
  border-radius: 6px;
  cursor: pointer;
}

.contents .upload-box .file {
  display: none;
}

.contents .files {
  width: calc(50% - 15px);
  box-sizing: border-box;
  overflow: auto;
  height: 360px;
}

.contents .files .file {
  display: flex;
  padding: 20px 20px;
  border-bottom: 1px solid #dbdbdb;
}

.contents .files .file:last-child {
  margin-bottom: 0px;
  border-bottom: none;
}

.contents .files .file .thumbnail {
  display: flex;
  flex: none;
  width: 50px;
  margin-right: 20px;
  align-items: center;
}

.contents .files .file .thumbnail .image {
  width: 100%;
}

.contents .files .file .details {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.contents .files .file .details .header {
  display: flex;
}

.contents .files .file .details .header .name {
  width: 100px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.contents .files .file .details .header .size {
  margin-left: auto;
}

.contents .files .file .progress {
  position: relative;
  height: 6px;
  background-color: #dbdbdb;
  overflow: hidden;
  margin-top: 4px;
  border-radius: 10px;
}

.contents .files .file .progress .bar {
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  background-color: #5b975b;
}

.contents .files .file .status {
  display: flex;
  width: 100%;
}

.contents .files .file .status .percent {}

.contents .files .file .status .speed {
  margin-left: auto;
}

@media(max-width: 700px) {
  .contents {
    display: flex;
    flex-direction: column;
    margin-top: 30px;
  }

  .contents .upload-box {
    width: 100%;
    box-sizing: border-box;
    margin-right: 0;
  }

  .contents .upload-box .drag-file {
    height: 150px;
  }

  .contents .files {
    width: 100%;
    box-sizing: border-box;
    margin-right: 0;
    overflow: initial;
  }
}
</style>

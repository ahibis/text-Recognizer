<script setup>
import { ref } from "vue"
import Tesseract from 'tesseract.js';

const lang = ref("eng")
const src = ref("")
const text = ref("")
function recognize(){
  if(!src.value) return;
  if(!lang.value) return;
  Tesseract.recognize(
    src.value,
    lang.value
  ).then(({ data: { text: recognizedText } }) => {
    text.value = recognizedText
  })
}
function imgChange(e) {
  const file = e.srcElement.files[0]
  if(!file) return;
  const url = URL.createObjectURL(file)
  src.value = url;
  recognize()
}
function imgPut(e){
  const {target} = e;
  const img = e.target.getElementsByTagName("img")[0];
  console.log(img)
  if(!img) return
  src.value = img.src;
  target.innerHTML = "вставьте сюда картинку"
  recognize()
}
</script>
<template>
  <div class="table">
    <div>
      язык:
      <select v-model="lang" @change="recognize">
        <option value="eng">English</option>
        <option value="rus">Russian</option>
      </select>
      картнка:
      <input type="file" accept=".png,.jpg" @change="imgChange">
      <div>
      <div @input="imgPut" contenteditable="true" style="text-align: left;">вставьте сюда картинку</div>
      </div>
      
      <img :src="src" v-if="src" style="width:100%">
    </div>
    <div>
      {{ text }}
    </div>
  </div>
</template>
<style>
.table {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
</style>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import { marked } from 'marked';
import CrudIndex from './Home/CrudIndex';
import LoadBox from '../components/LoadBox.vue'
let initDisplay = ref(false);
let answer = ref("");
/**
*
* @param
*
* @return
*/
async function sendText(){
  try {
    initDisplay.value = true;
    answer.value = "";
    const result = await CrudIndex.addItem();
console.log(result);
    initDisplay.value = false;
    if(result) { 
      const s = marked.parse(result);      
console.log(s); 
      answer.value = s.toString(); 
    } 
  } catch (error) {
    initDisplay.value = false;
    console.error(error);
  }    
}
/**
*
* @param
*
* @return
*/
async function clearText(){
  try {
    const input_text = (<HTMLInputElement>document.querySelector("#input_text"));
    if(input_text){
      input_text.value = "";
    }
  } catch (error) {
      console.error(error);
  }    
}
</script>

<template>
  <div v-if="initDisplay"><LoadBox />
  </div>
  <div class="container mx-auto my-2 px-8 bg-white">
    <h1 class="text-4xl font-bold">AI-Chat!</h1>
    <hr class="my-2">
    <div class="flex flex-row">
      <div class="flex-1 text-center p-1">
        <textarea
         class="border border-gray-400 rounded-md px-3 py-2 w-full resize-none focus:outline-none focus:border-blue-500"
         name="input_text" id="input_text" rows="3" />
      </div>
      <div class="text-center pt-1">
        <button 
        class="bg-blue-600 text-white font-bold ms-2 py-2 px-4 rounded"
        @click="sendText" >Send</button>
        <br />
        <button 
        class="bg-transparent hover:bg-blue-500 text-blue-700 font-semibold hover:text-white ms-2 mt-2 py-2 px-4 border border-blue-500 hover:border-transparent rounded"
        @click="clearText">Clear</button>        
      </div>
    </div>
    <hr class="my-2">
    answer :<br />
    <div v-if="answer" class="bg-sky-100 p-2 rounded">
      <span v-html="answer"></span>
    </div>

  </div>
</template>

<style scoped>
</style>

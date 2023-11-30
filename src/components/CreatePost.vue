<script setup>
import {ref} from 'vue'
import { collection, query, where, getDocs, onSnapshot, addDoc, serverTimestamp } from "firebase/firestore"
import db from "../firebase/init.js"

const body = ref("")

const props = defineProps(['user'])

const sendPost = async () => {
  if(body.value!==""){
    const commentRef = collection(db, "posts");
    const addPost = await addDoc(commentRef, {
      body: body.value,
      postdate: serverTimestamp(),
      user: props.user
    })
    console.log(addPost)
  }
}

</script>
<template>

    <div class=" box">
        <div>
          Create a New Post 
        </div>
        <div class="flex">
            <input type="text" style="width: 500px;" id="bb" class="input" required placeholder="Enter a post message" v-model="body"><br>
             <input type="submit" class="send"  @click="sendPost" value="Create">
        </div>
      </div>
     
</template>

<style scoped>

.box {
  border: 1px solid grey;
  padding: 5px;
  margin: 5px;
  border-radius: 5px;
  background-color: rgba(177, 177, 164, 0.322);
}

.title {
  font-style: italic;
  color: hsla(160, 100%, 37%, 1);
}

.send{
    width: 100px;

margin-left: 5px;    
padding: 5px 10px;
    color: azure;
    border-radius: 5px;
    border-width: 2px;
    border-color: black;
    background-color: rgb(37, 58, 116);
    cursor: pointer;
}

.flex {
  display: flex block ;
  font-style: italic;
  color: hsla(160, 100%, 37%, 1);
}

label{
    display: inline-block;
    vertical-align: text-top;
    border: 1px;
}

.input{
    border-radius: 5px;
    border-width: 1px;
    border-color: black;
}
input{
    width: auto;
}


.newbtn {
  color: rgb(80, 135, 117);
  border: cadetblue;
  border: 2pt;
  cursor: pointer;
  background-color: blanchedalmond;
  padding: 2px 5px;
  border-radius: 4px;
}
</style>
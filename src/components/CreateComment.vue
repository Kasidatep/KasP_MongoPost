<script setup>
import {ref} from 'vue'
import { collection, query, where, getDocs, onSnapshot, addDoc, serverTimestamp } from "firebase/firestore"
import db from "../firebase/init.js"

const stars = ref(0)
const email = ref("")
const name = ref("")
const comment = ref("")

const emits = defineEmits(['add'])
const props = defineProps(['post'])
const sendComment = async () => {
    console.log(props.post)
    const commentRef = collection(db, "posts", props.post.id, "comments");
    const addComment = await addDoc(commentRef, {
        name: name.value,
        email: email.value,
        comment: comment.value,
        cmtdate: serverTimestamp(),
        stars: stars.value
    })

    emits('add')

}

</script>
<template>

    <div class="box">
        <div class="flex" style="padding: 5px;">
        <div>
          Write a New Comment 
        </div>
        <div class="newbtn" @click="isOpenCommentBox = false">
          close
        </div>
      </div>
   
      <label for="email">Email*: </label><input type="email" id="email" class="input" placeholder="Enter your email" v-model="email"><br>
      <label for="name">Name*: </label><input type="text" id="name" class="input"  placeholder="Enter your name" v-model="name"><br>     
      <label for="comment">Comment*: </label><textarea type="text" id="comment" class="input"  placeholder="Write a comment" v-model="comment"> </textarea><br>     
      <label for="Stars">Stars: [{{ stars }}] </label><input type="range" id="Stars" placeholder="Give a score" style="animation: both;" class="input" min="0" max="5" v-model="stars"><br>   
      <div class="send" @click="sendComment">Send ></div>
    
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
    width: 70px;
    padding: 5px 10px;
    color: azure;
    border-radius: 5px;
    border-width: 2px;
    border-color: black;
    background-color: rgb(37, 58, 116);
    cursor: pointer;
}

.flex {
  display: flex;
  justify-content: space-between;
  font-style: italic;
  color: hsla(160, 100%, 37%, 1);
}

label{
    width: 80px;
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
    min-width: 50%;
}

textarea{
    margin-top: 5px;
    width: auto;
    min-width: 50%;
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
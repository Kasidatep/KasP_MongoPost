<script setup>
import {ref} from 'vue'
import { collection, query, where, getDocs, onSnapshot, addDoc, serverTimestamp } from "firebase/firestore"
import db from "../firebase/init.js"

const firstname = ref(null)
const lastname = ref(null)
const dob = ref(null)
const sendUser = async () => {
  if(firstname.value!==null && lastname.value!==null && dob.value!==null){
    console.log(dob.value)
    const commentRef = collection(db, "users");
    const addComment = await addDoc(commentRef, {
      firstname: firstname.value,
      lastname: lastname.value,
      dob: new Date(dob.value),
    })
    firstname.value = null
    lastname.value = null
    dob.value = null

  }
 

}

</script>
<template>

    <div class="box">
        <div class="flex" style="padding: 5px;">
        <div>
          Create a New User 
        </div>
        <!-- <div class="newbtn" @click="isOpenCommentBox = false">
          close
        </div> -->
      </div>
      <label for="fname">Fristname*: </label><input type="text" id="fname" class="input" required placeholder="Enter your fristname" v-model="firstname"><br>
      <label for="lname">Lastname*: </label><input type="text" id="lname" class="input" required placeholder="Enter your lastname" v-model="lastname"><br>     
      <label for="db">DoB*: </label><input type="date" v-model="dob" required id="db"><br>     
      <input type="submit" class="send" @click="sendUser" value="Create">
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
    width: 80px;
    margin-top: 10px;
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
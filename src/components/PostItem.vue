<script setup>
import { ref } from "vue";
import CommentItem from "../components/CommentItem.vue"
import CreateComment from "../components/CreateComment.vue";
const isOpenCommentBox = ref(false)
defineProps({
  post: {
    type: Object,
    required: true,
  }
});

defineEmits(['add'])


</script>

<template>
  <div class="box">
    <div class="flex">
      <div>
        {{ post.body }}
      </div>
      <div class="newbtn" @click="isOpenCommentBox = !isOpenCommentBox">
        New Comment
      </div>
    </div>


    <h4 class="title">comments ({{ post.comments.length }})</h4>
    <CommentItem v-for="comment in post.comments" :comment="comment" :key="comment.id" />

    <div v-if="isOpenCommentBox">
      <hr>
     <CreateComment :post="post" @add="$emit('add')" />  
    </div>


    
  </div>
</template>

<style scoped>
.box {
  border: 1px solid grey;
  padding: 5px;
  margin: 5px;
  border-radius: 5px;
}

.title {
  font-style: italic;
  color: hsla(160, 100%, 37%, 1);
}

.flex {
  display: flex;
  justify-content: space-between;
  font-style: italic;
  color: hsla(160, 100%, 37%, 1);
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



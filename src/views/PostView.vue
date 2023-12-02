<script setup>
import { ref, onMounted, watch } from 'vue'
import { useRoute } from "vue-router"
import { collection, query, where, getDocs, onSnapshot, doc, getDoc } from "firebase/firestore"
import db from "../firebase/init.js"
import PostItem from "../components/PostItem.vue"
import CreatePostVue from '../components/CreatePost.vue'
const username = ref("")
const user = ref(null)
const posts = ref([])
const route = useRoute()
const count = ref(0)

async function getPosts() {
  username.value = route.params.user
  user.value = undefined
  const u = await getDoc(doc(db, "users", username.value))
  user.value = u.data()

  posts.value = []
  const postRef = collection(db, "posts");
  const qry = query(postRef, where("user", "==", username.value));
  const unsubscribe = onSnapshot(qry, (querySnap) => {
    posts.value = []
    count.value = 0      
    console.log(querySnap)
    count.value = querySnap.size
    querySnap.forEach(async (doc) => {
       let data = doc.data();
      data.id = doc.id;
      const commentRef = collection(db, "posts", doc.id, "comments");
      const queryComment = await getDocs(query(commentRef));
      console.log(queryComment)
      
      data.comments = []
      queryComment.forEach((com) => {
        let comment = com.data();
        comment.id = com.id;
        console.log(comment)
        data.comments.push(comment);
      })
      posts.value.push(data);
    });
  })
}

watch(() => route.params.user, getPosts)

const add = () => {
  getPosts()
}

onMounted(() => {
  getPosts()
})

</script>

<template>
  <h3>Posts : {{ user?.firstname }} {{ user?.lastname }} <span class="brd">{{ count }} post</span> </h3>
  <CreatePostVue :user="user" />
  <PostItem v-for="post in posts" :post="post" :key="post.id" @add="add" />
</template>

<style scoped>
.brd{
  background-color: black;
  color: aliceblue;
  padding: 5px 10px;
  border-radius: 10px;
  font-size: 12px;
  font-weight: 700;
}
</style>


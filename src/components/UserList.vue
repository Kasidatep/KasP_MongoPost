<script setup>
import { RouterLink, RouterView } from 'vue-router'
import CreateUser from './CreateUser.vue';
defineProps({
  users: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div class="greetings">
    <h3>User List</h3>
    <ul>
        <li v-for="user in users" :key="user.id">
            <RouterLink :to="`/posts/${user.id}`">{{user?.firstname}} {{ user?.lastname }}</RouterLink>
        </li>
        
          <CreateUser />
        
    </ul>
  </div>
</template>

<style scoped>

</style>

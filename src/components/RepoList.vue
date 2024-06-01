
<script setup>
import { defineProps } from 'vue'
defineProps({
  repos: {
    type: Array,
    required: true,
    default() {
      return []
    }
  }
})
</script>

<template>
  <section>
    <h2>Repo List</h2>
    <p v-if="!repos.length">No repositories found.</p>

    <ul>
      <li class="repo-list" v-for="repo in repos" :key="repo.id">
        <router-link
          :to="{ name: 'SingleRepoView', params: { id: repo.name, name: repo.owner.login } }"
        >
          {{ repo.name }}
        </router-link>
        <!-- <a :href="repo.html_url">{{ repo.name }}</a> -->
      </li>
    </ul>
  </section>
</template>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
  margin-block: 10px 50px;
  display: flex;
  width: 100vw;
  overflow: auto;
}

.repo-list{
  border: whitesmoke solid 1px;
  width: max-content;
  margin-block: 8px;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 1px 1px #ccc
}
</style>

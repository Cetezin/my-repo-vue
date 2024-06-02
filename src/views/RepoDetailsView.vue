<script setup>
defineProps(['repo'])
</script>

<template>
  <div>
    <h1>Repo Details</h1>
   <div class="main">
    <p>Repo Owner: {{ repo.owner.login }}</p>
    <p>Repo Description: {{ repo.description || "none"}}</p>
    <p>Language: {{ repo.language || "unknown" }}</p>
    <p>Created: {{repo.created_at}}</p>
    <p>Default Branch: {{repo.default_branch}}</p>
   </div>
  </div>
</template>

<style scoped>
  p{
    border-inline: #ccc solid 1px;
    padding-inline: 15px;
    color: rgb(157, 157, 212);
    margin-bottom: 8px;
  }

  .main{
    border-block: #ccc solid 1px;
    margin-top: 20px;
    background: whitesmoke;
    box-shadow: rgb(235, 230, 230) 1px 1px 0
  }
</style>
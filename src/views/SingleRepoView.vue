<script setup>
defineProps(["repo"]);
</script>

<template>
  <div id="main">
    <h1>Single Repo</h1>
    <hr>
    <br>
    <p>Repo ID: {{ repo.name }}</p>
  </div>
</template>

<style scoped>
#main {
  margin-top: 20px;
  border: 1px solid #ccc;
  padding: 15px;
  background: whitesmoke;
}

#main h1{
  color: rgb(248, 131, 89)
}
</style>

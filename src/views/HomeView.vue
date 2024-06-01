<script setup>
import { onMounted, reactive, ref, watchEffect, computed } from "vue";
import api from "../services/api";
import RepoList from "../components/RepoList.vue";
import Pagination from "../components/RepoPagination.vue";
import { parseLinkHeader, extractPageNumber } from "@/utils";

const username = ref("cetezin");
const github = reactive({
  user: null,
  repos: [],
  links: null,
  totalPages: 0,
  currentPage: 1,
});

const props = defineProps(["page"]);

const page = computed(() => props.page);

const hasNextPage = computed(() => {
  const totalPages = github.totalPages;
  return page.value < totalPages;
});

function fetchRepos(page) {
  api.getRepos(username.value, { page }).then((repos) => {
    // github.totalPages = extractPageNumber(parsedLink.last)
    github.repos = repos.data;
    github.links = repos.headers.link;
  });
}

onMounted(() => {
  watchEffect(() => {
    api.getUser(username.value).then((user) => {
      github.user = user.data;
    });
    api.getRepos(username.value, { page: page.value }).then((repos) => {
      const parsedLink = parseLinkHeader(repos.headers.link);
      github.totalPages = extractPageNumber(parsedLink?.last) || page;
      github.repos = repos.data;
      github.link = repos.headers.link;
    });
  });
});
</script>

<template>
  <section id="container">
    <form @submit.prevent="">
      <label for="username">Enter a GitHub username: </label>
      <input
        style="color: #ccc; width: 30%"
        v-model.lazy="username"
        type="text"
        id="username"
      />
      <!-- <button type="submit">Submit</button> -->
    </form>

    <p v-if="!github">Loading...</p>

    <div v-if="github.user">
      <div class="profile" >
        <img
          :src="github.user.avatar_url"
          style="border-radius: 50%; width: 120px; margin: 30px 10px"
          id="image"
          alt="User avatar"
        />
        <div class="title">
          <h3>{{ github.user.login }}</h3>
          <p>{{  github.user.bio }}</p>
          <p>{{  github.user.email}}</p>
        </div>
      </div>

      <div class="pagination">
        <router-link
          id="page-prev"
          :to="{ name: 'Home', query: { page: page - 1 } }"
          rel="prev"
          v-if="page != 1"
          ><span id='prev'>&#60;</span> Previous</router-link
        >

        <router-link
          id="page-next"
          :to="{ name: 'Home', query: { page: page + 1 } }"
          rel="next"
          v-if="hasNextPage"
          >Next <span id="next">&#62;</span></router-link
        >
      </div>

      <RepoList :repos="github.repos" :totalPages="github.totalPages" />

      <Pagination
        :totalPages="github.totalPages"
        @fetchRepo="(n) => fetchRepos(n)"
      />
    </div>
  </section>
</template>

<style scoped>

#username {
  width: 80%;
  border: #ccc solid 2px;
  font-size: 13px;
  margin-top: 10px;
  padding: 5px;
}

#username:focus {
  border: solid rgb(54, 29, 80);
}

#prev, #next{
    border: #969595 solid 1px;
    padding-inline: 3px;
    padding-block-end: 1px;
    border-radius: 2px;
    background: whitesmoke;
    color: #a19e9e;
    font-weight: bolder;
}

.title h3{
  font-weight: bold
}

.title p{
  font-size: 13px;
  color: #a19e9e;
}

.profile{
    display: flex;
    align-items: center
}

.pagination {
  display: flex;
  justify-content: space-between
}
.pagination a {
  flex: 1;
  text-decoration: none;

  &:hover {
    text-decoration: underline;
    background-color: transparent;
  }
}
</style>

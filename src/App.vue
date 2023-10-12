<script setup  >
import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";
import { ref, computed, onMounted } from "vue";

const posts = ref([]);
const favorito = ref(' ');
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(false);

const cambiarFavorito = (title) => {
  favorito.value = title;
}

const next = () => {
  inicio.value += + postXpage;
  fin.value += + postXpage;

}
const prev = () => {
  inicio.value += - postXpage;
  fin.value += - postXpage;

}

onMounted (async() => {
  loading.value = true;
  try{
    const res = await fetch ('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
    }catch(error){
      console.log(error)
  } finally{
    loading.value = false
  }

})

/*
fetch('https://jsonplaceholder.typicode.com/posts')
  .then(res => res.json())
  .then((data) => { posts.value = data; })
  .catch((e) => console.log(e))
  .finally(() => {
    setTimeout(() => {
      loading.value = false
    }, 500)

  });

*/
const maxLength = computed(() => posts.value.length)

</script>

<template>
  <LoadingSpinner v-if="loading" />

  <div class="container" v-else></div>


  <h1> APP </h1>
  <h2>Mi post favorito: {{ favorito }}</h2>


  <PaginatePost class="mb-2" @next="next" @prev="prev" :inicio="inicio" :fin="fin" :maxLength="maxLength" />


  <BlogPost v-for="post in posts.slice(inicio, fin)" :key="post.id" :title="post.title" :id="post.id" :body="post.body"
    @cambiarFavoritoNombre="cambiarFavorito" class="mb-2"></BlogPost>
</template>
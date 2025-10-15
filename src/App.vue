<script setup lang="ts">
import { ref } from 'vue'

import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'

const randomPokemon = ref({
  name: '',
  sprites: {
    front_default: null
  }
})

async function getData() {
  const url = `https://pokeapi.co/api/v2/pokemon/${Math.floor(Math.random() * 500)}`

  const response = await fetch(url)
  const result = await response.json()

  randomPokemon.value = result
  console.log('result', result)

  if (!response.ok) {
    throw new Error(`Response status: ${response.status}`)
  }
}

console.log('randomPokemon', randomPokemon)

// const imageUrl = ref('https://unsplash.com/s/photos/cats')
</script>

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->
    <!-- <h1>This is gonna be permanent</h1> -->
    <p>{{ randomPokemon.name }}</p>
    <button @click="getData">This is my cool button</button>
    <img
      v-if="randomPokemon.sprites.front_default"
      :src="randomPokemon.sprites.front_default"
      width="300"
      height="300"
    />

    <!-- <div class="wrapper">
      <HelloWorld msg="Hello World" />
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div> -->
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>

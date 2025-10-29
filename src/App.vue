<script setup lang="ts">
import { ref } from 'vue'

import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'

const randomPokemon = ref({
  name: '',
  sprites: {
    front_default: null,
  },
})

const pokemonId = ref(1)

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

async function getDataFromDjangoAPI(pokemonId:any) {
  const url = `http://127.0.0.1:8000/api/pokemon/${pokemonId}/`

  try {
    const response = await fetch(url, {
      headers: {
        'Content-Type': 'application/json',
      },
    })

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const result = await response.json()
    randomPokemon.value = result
    console.log('Pokémon data:', result)
  } catch (error) {
    console.error('Fetch failed:', error)
  }
}

console.log('Pokemon', randomPokemon)

// const imageUrl = ref('https://unsplash.com/s/photos/cats')
</script>

<template>
  <header>
    <!-- <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" /> -->
    <!-- <h1>Calling the <a href="https://pokeapi.co/">pokiAPI</a></h1>
    <p>{{ randomPokemon.name }}</p>
    <button @click="getData">This is my cool button</button>
    <img
      v-if="randomPokemon.sprites.front_default"
      :src="randomPokemon.sprites.front_default"
      width="300"
      height="300"
    /> -->

    <!-- <div class="wrapper">
      <HelloWorld msg="Hello World" />
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div> -->
  </header>

  <!-- <RouterView /> -->

  <div>
    <h1>Calling custom django poke API</h1>
    <label for="id">
      Enter the pokemon id
      <input name="id" type="number" v-model.number="pokemonId" min="1" />
    </label>
    <div>
      <button @click="getDataFromDjangoAPI(pokemonId)">Get my pokemon from Django API pokedex</button>
    </div>
    <br />
    <h1>Pokemon name</h1>
    <p>{{ randomPokemon.name }}</p>
  </div>
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

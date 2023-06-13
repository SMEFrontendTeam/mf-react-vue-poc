<template>
  <div>
    <h1 class="text-center font-bold text-3xl">DB MF</h1>

    <div class="bg-white">
      <div
        class="mx-auto max-w-2xl py-16 px-4 sm:py-24 sm:px-6 lg:max-w-7xl lg:px-8"
      >
        <div
          class="grid grid-cols-1 gap-y-10 gap-x-6 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 xl:gap-x-8"
        >
          <div v-for="personaje in personajes" :key="personaje.id">
            <a :href="`/pokemon/${personaje.name}`">
              <div
                class="aspect-w-1 aspect-h-1 w-full overflow-hidden rounded-lg bg-gray-200 xl:aspect-w-7 xl:aspect-h-8"
              >
                <img
                  :src="personaje.imgUrl"
                  :alt="personaje.name"
                  height="150"
                  class="w-full object-cover object-center group-hover:opacity-75"
                />
              </div>
              <h3 class="mt-4 text-sm text-gray-700">
                Peso: {{ personaje.weight }}
              </h3>
              <p class="mt-1 text-lg font-medium text-gray-900">
                {{ personaje.name.charAt(0).toUpperCase() + personaje.name.slice(1)}}
              </p>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";

const personajes = ref([]);

const getPersonajes = async () => {
  const res = await fetch(
    "https://pokeapi.co/api/v2/pokemon/"
  );
  const data = await res.json();

  const salida = data.results;

 salida.map((e)=>{
    fetch(`https://pokeapi.co/api/v2/pokemon/${e.name}`).then(response => response.json()).then((json) => {
      const returnPokeItem={
        name:json.name,
        weight:json.weight,
        id:json.id,
        imgUrl:json.sprites.front_default
      }
      personajes.value.push(returnPokeItem)

    })
  })
};

onMounted(() => {
  getPersonajes();
});
</script>

<script setup lang="ts">
  import { ref } from "vue";
  import heroes from "./heroes.json"

  interface Hero {
    "Hero" : string,
    "Type" : string,
    "Icon" : string,
  }

  const currentHero = ref<Hero>({
    "Hero":"",
    "Type":"",
    "Icon":"",
  })

  const lastHeroes = ref<Hero[]>([])

  function randomHero() {

    if(currentHero.value.Hero !== "") {
      if(lastHeroes.value.length === 3) {
        lastHeroes.value.reverse().pop()
        lastHeroes.value.reverse().push({...currentHero.value})
      } else {
        lastHeroes.value.push({...currentHero.value})
      }
    }

    

    let random = Math.floor(Math.random() * heroes.length)

    for(let i=0 ; i<lastHeroes.value.length ; i++) {
      let ok = false
      while(ok === false) {
        if (heroes[random].Hero === lastHeroes.value[i].Hero) {
          random = Math.floor(Math.random() * heroes.length)
        } else {
          ok = true
        }
      }
    }

    currentHero.value = heroes[random]

  }

</script>

<template>
  <div class="min-h-screen min-w-screen bg-cover bg-[url(/background.jpg)]">
    <div class="h-screen w-screen bg-black/50 bg-o flex flex-col gap-4 justify-center items-center">
        <button
        class="border rounded-lg bg-white text-2xl p-2 cursor-pointer"
        @click="randomHero()"
        >
        Give me a random Hero!
      </button>
      
      <div class="flex flex-col gap-2">
        <p class="text-black text-2xl">Last 3 Heroes:</p>
        <p class="text-black text-2xl" v-for="hero in lastHeroes" :key="hero.Hero">{{ hero.Hero }}</p>
      </div>
      
      <div v-if="currentHero.Hero !== ''">
        <img :src="/icons/ + currentHero.Icon" :alt="currentHero.Hero" />
      </div>
    </div>
  </div>
</template>



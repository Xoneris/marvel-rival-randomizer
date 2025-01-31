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

  function randomHero(type:string) {

    let heroArray

    if (type !== "") {
      heroArray = heroes.filter((hero) => hero.Type === type)
    } else {
      heroArray = heroes
    }

    if (currentHero.value.Hero !== "") {
      if (lastHeroes.value.length >= 3) {
        lastHeroes.value.shift();
      }
      lastHeroes.value.push({ ...currentHero.value });
    }

    let random:number

    do {
      random = Math.floor(Math.random() * heroArray.length);
    } while (lastHeroes.value.some((hero) => hero.Hero === heroArray[random].Hero));

    currentHero.value = heroArray[random] 
  }

</script>

<template>
  <div class="min-h-screen min-w-screen bg-cover bg-[url(/background.jpg)]">
    <div class="h-screen w-screen bg-black/50 flex justify-center items-center">

      <div class="h-[700px] flex flex-col gap-4 items-center">

        <button
          class="border rounded-lg bg-white text-2xl p-2 cursor-pointer"
          @click="randomHero('')"
          >
          Give me a random Hero!
        </button>
      
        <div class="flex gap-1">
          <button
            class="border rounded-lg bg-white text-xl p-2 cursor-pointer"
            @click="randomHero('Vanguard')"
            >
            Random Vanguard!
          </button>
          <button
          class="border rounded-lg bg-white text-xl p-2 cursor-pointer"
          @click="randomHero('Duelist')"
          >
            Random Duelist!
          </button>
          <button
            class="border rounded-lg bg-white text-xl p-2 cursor-pointer"
            @click="randomHero('Strategist')"
            >
            Random Strategist!
          </button>
        </div>

        
        <div v-if="currentHero.Hero !== ''">
          <img :src="/icons/ + currentHero.Icon" :alt="currentHero.Hero" />
        </div>
        
        <div v-if="lastHeroes.length > 0" class="flex flex-col gap-2 p-2 rounded-lg bg-white/50">
          <p class="text-black text-2xl">Last 3 Heroes:</p>
          <p class="text-black text-xl" v-for="hero in lastHeroes" :key="hero.Hero">{{ hero.Hero }}</p>
        </div>
        
      </div>
    </div>
  </div>
</template>



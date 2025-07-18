<template>
  <div>
    <div class="w-full hidden md:flex justify-between items-center py-2 xl:px-16 md:px-8 lg:px-20 border-b top-0 fixed bg-[#5f259f] text-white z-50 xl:text-[16px] lg:text-[14px] md:text-[14px]">

      <div>
        <NuxtImg src="../public/img/logo.png" alt="logo de l'application" class="xl:w-[70px] lg:w-[70px] md:w-[70px]" ></NuxtImg>
      </div>

      <div class="flex flex-row gap-4 items-center uppercase">
        <NuxtLink to="/">
          <p>Acceuil</p>
        </NuxtLink>
        <NuxtLink to="/Equipements-et-activites">
          <p>équipements et activités</p>
        </NuxtLink>
        <NuxtLink to="/Espaces-verts">
          <p>espaces verts</p>
        </NuxtLink>
        <NuxtLink to="/Fontaines-a-boire">
          <p>Fontaines à boire</p>
        </NuxtLink>
      </div>
    </div>

    <div class="w-full md:hidden fixed top-0 left-0 bg-[#5f259f] text-white z-50 px-4 py-4 ">
      <div class="flex justify-between items-center">

        <div>
          <NuxtImg src="../public/img/logo.png" alt="logo de l'application" class="w-[60px]" ></NuxtImg>
        </div>

        <button @click="isOpen = !isOpen">
          <svg
            v-if="!isOpen"
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      
      <div v-if="isOpen" class="flex flex-col justify-between gap-4 mt-4 uppercase text-sm h-full">
        <hr class="text-white">
        <NuxtLink to="/">
          <p>Accueil</p>
        </NuxtLink>
        <NuxtLink to="/Equipements-et-activites" @click="isOpen = false">
          équipements et activités
        </NuxtLink>
        <NuxtLink to="/Espaces-verts" @click="isOpen = false">
          ésaces verts
        </NuxtLink>
        <NuxtLink to="/Fontaines-a-boire" @click="isOpen = false">
          Fontaines à boire
        </NuxtLink>
      </div>

    </div>
  </div>
</template>

<script setup>
import { NuxtLink } from '#components';
import { ref } from 'vue'
const isOpen = ref(false)
</script>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuery } from '@tanstack/vue-query';

const firstName = ref('');
const lastName = ref('');

const submitForm = () => {
  console.log('Prénom:', firstName.value);
  console.log('Nom:', lastName.value);
};

</script>

<template>
  <div class="">
    <div class="bg-white p-8 rounded-lg shadow-md">
      <h1 class="text-2xl text-gray-700 font-bold mb-4">Formulaire de réservation</h1>
      <form @submit.prevent="submitForm">
        <div class="mb-4">
          <label for="firstName" class="block text-gray-700 font-bold mb-2">Prénom</label>
          <input type="text" id="firstName" v-model="firstName" placeholder="Entrez votre prénom" class="w-full px-3 py-2 text-gray-500 border rounded-md focus:outline-none focus:border-blue-500">
        </div>
        <div class="mb-4">
          <label for="lastName" class="block text-gray-700 font-bold mb-2">Nom</label>
          <input type="text" id="lastName" v-model="lastName" placeholder="Entrez votre nom" class="w-full px-3 py-2 text-gray-500 border rounded-md focus:outline-none focus:border-blue-500">
        </div>
        <div class="text-center">
          <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600">Valider</button>
        </div>
      </form>
    </div>
    <!-- Affichez les données récupérées par la requête de test
    <div v-if="isLoading">Chargement...</div>
    <div v-else-if="isError">Erreur lors du chargement des données.</div>
    <div v-else>
      <h2>Données récupérées:</h2>
      <pre>{{ data }}</pre>
    </div> -->
  </div>
</template>

<style scoped>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>

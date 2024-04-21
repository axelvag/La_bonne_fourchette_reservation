<script setup lang="ts">
import { useQuery } from '@tanstack/tanquery';

const { data, isLoading, isError } = useQuery('exampleQuery', async () => {
  const response = await fetch('https://api.example.com/data');
  if (!response.ok) {
    throw new Error('Erreur lors de la récupération des données');
  }
  return response.json();
});
</script>

<template>
  <div>
    <div v-if="isLoading">Chargement...</div>
    <div v-else-if="isError">Erreur lors du chargement des données.</div>
    <div v-else>
      <h2>Données récupérées:</h2>
      <pre>{{ data }}</pre>
    </div>
  </div>
</template>

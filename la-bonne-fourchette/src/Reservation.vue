<script setup lang="ts">
import { ref } from 'vue';
import { useQuery } from '@tanstack/vue-query';
import { validateDateFormat, validateHeureFormat, validateNameFormat, validatePhoneNumberFormat } from '@/utils/Form-error.vue';

const Date = ref('');
const Heure = ref('');
const FirstName = ref('');
const LastName = ref('');
const Email = ref('');
const PhoneNumber = ref('');
const reservationMessage = ref('');
const acceptPub = ref(false);


const submitForm = () => {
  if (!validateDateFormat(Date.value)) {
    console.error('Format de date invalide. Le format correct est YYYY-MM-DD.');
    return;
  }
  else if (!validateHeureFormat(Heure.value))
  {
    console.error('Format de l\'heure invalide. Le format correct est HH:MM.');
    return;
  }
  // fetchDataValider(Date.value, 'la-bonne-fourchette');
};

const submitFormInfo = () => {
  if (FirstName.value === '' || LastName.value === '' || Email.value === '' || PhoneNumber.value === '')
  {
    console.error('Veuillez remplir tout les champs vide.');
    return;
  }
  if (!validateNameFormat(FirstName.value)) {
    console.error('Format du prénom invalide. Le prénom ne doit pas contenir de chiffres.');
    return;
  }
  else if (!validateNameFormat(LastName.value))
  {
    console.error('Format du nom invalide. Le nom ne doit pas contenir de chiffres.');
    return;
  }
  else if (validatePhoneNumberFormat(PhoneNumber.value))
  {
    console.error('Format du numéro de téléphone invalide. Le numéro de téléphone doit contenir seulement de nombres.');
    return;
  }
  else if (Date.value === '' || Heure.value === '' || !validateDateFormat(Date.value) || !validateHeureFormat(Heure.value))
  {
    console.error('Veuillez remplir un horaire disponible.');
    return;
  }
  reservationMessage.value = 'Réservation envoyée';
  // fetchDataReserver(FirstName.value,
  //   LastName.value,
  //   Email.value,
  //   PhoneNumber.value,
  //   Date.value,
  //   Heure.value,
  //   acceptPub.value);
};

const fetchDataValider = async (date, restaurant) => {
  console.log('Fetching data for:', date, restaurant);
  try {
    const response = await fetch(`https://api.test.book-eat.fr/openings?date=${date}&name=${restaurant}`);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

const fetchDataReserver = async (
  firstName: string,
  lastName: string,
  email: string,
  phoneNumber: string,
  date: string,
  time: string,
  acceptPub: boolean,
) => {
  try {
    const response = await fetch('https://api.test.book-eat.fr/bookings/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        first_name: firstName,
        last_name: lastName,
        email: email,
        phone: phoneNumber,
        date: date,
        time: time,
        nb_person: 2,
        comment: '',
        opening: 1,
        acceptPub: acceptPub,
        acceptCGU: true,
      }),
    });
    if (!response.ok) {
      throw new Error('Failed to submit reservation');
    }
    console.log('Réservation soumise avec succès');
  } catch (error) {
    console.error('Erreur lors de la soumission de la réservation :', error);
  }
};

</script>

<template>

<!-- LEFT -->

  <div class="background-container flex justify-center items-center flex-col">
    <!-- Texte "La bonne fourchette" -->
    <span class="i-dashicons-food text-6xl text-center"></span>
    <h1 class="text-4xl text-white font-bold mb-8">La Bonne Fourchette</h1>

    <!-- Formulaire de réservation transparent -->
    <div class="glass-effect p-8 rounded-lg shadow-md">
      <h1 class="text-2xl text-white text-center font-serif font-bold mb-4">Choisir une date :</h1>
      <form @submit.prevent="submitForm">
        <div class="mb-4">
          <label for="Date" class="block text-gray-700 font-bold mb-2">Date</label>
          <input type="text" id="Date" v-model="Date" placeholder="YYYY-MM-DD" class="w-full px-3 py-2 text-gray-500 border rounded-md focus:outline-none focus:border-blue-500">
        </div>
        <div class="mb-4">
          <label for="Heure" class="block text-gray-700 font-bold mb-2">Heure</label>
          <input type="text" id="Heure" v-model="Heure" placeholder="HH:MM" class="w-full px-3 py-2 text-gray-500 border rounded-md focus:outline-none focus:border-blue-500">
        </div>
        <div class="text-center">
          <button type="submit" class="bg-yellow-500 text-white px-4 py-2 rounded-md hover:bg-yellow-600">Valider</button>
        </div>
      </form>
    </div>

  </div>


<!-- RIGHT -->

  <div class="isolate bg-white px-6 py-24 sm:py-32 lg:px-8">
  <div class="absolute inset-x-0 top-[-10rem] -z-10 transform-gpu overflow-hidden blur-3xl sm:top-[-20rem]" aria-hidden="true">
    <div class="relative left-1/2 -z-10 aspect-[1155/678] w-[36.125rem] max-w-none -translate-x-1/2 rotate-[30deg] bg-gradient-to-tr from-[#ff80b5] to-[#9089fc] opacity-30 sm:left-[calc(50%-40rem)] sm:w-[72.1875rem]" style="clip-path: polygon(74.1% 44.1%, 100% 61.6%, 97.5% 26.9%, 85.5% 0.1%, 80.7% 2%, 72.5% 32.5%, 60.2% 62.4%, 52.4% 68.1%, 47.5% 58.3%, 45.2% 34.5%, 27.5% 76.7%, 0.1% 64.9%, 17.9% 100%, 27.6% 76.8%, 76.1% 97.7%, 74.1% 44.1%)"></div>
  </div>
  <div class="mx-auto max-w-2xl text-center">
    <h2 class="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">Informations Personnelles</h2>
    <p class="mt-2 text-lg leading-8 text-gray-600">Dernière étape avant de diner</p>
  </div>
  <form @submit.prevent="submitFormInfo" class="mx-auto mt-16 max-w-xl sm:mt-20">
    <div class="grid grid-cols-1 gap-x-8 gap-y-6 sm:grid-cols-2">
      <div>
        <label for="first-name" class="block text-sm font-semibold leading-6 text-gray-900">Prénom</label>
        <div class="mt-2.5">
          <input type="text" name="first-name" id="first-name" v-model="FirstName" autocomplete="given-name" class="block w-full rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
        </div>
      </div>
      <div>
        <label for="last-name" class="block text-sm font-semibold leading-6 text-gray-900">Nom</label>
        <div class="mt-2.5">
          <input type="text" name="last-name" id="last-name" v-model="LastName" autocomplete="family-name" class="block w-full rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
        </div>
      </div>
      <div class="sm:col-span-2">
        <label for="email" class="block text-sm font-semibold leading-6 text-gray-900">Email</label>
        <div class="mt-2.5">
          <input type="email" name="email" id="email" v-model="Email" autocomplete="email" class="block w-full rounded-md border-0 px-3.5 py-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
        </div>
      </div>
      <div class="sm:col-span-2">
        <label for="phone-number" class="block text-sm font-semibold leading-6 text-gray-900">Numéro de téléphone</label>
        <div class="relative mt-2.5">
          <div class="absolute inset-y-0 left-0 flex items-center">
            <label for="country" class="sr-only">Country</label>
            <select id="country" name="country" class="h-full rounded-md border-0 bg-transparent bg-none py-0 pl-4 pr-9 text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm">
              <option>FR</option>
              <option>UK</option>
              <option>US</option>
            </select>
            <svg class="pointer-events-none absolute right-3 top-0 h-full w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
              <path fill-rule="evenodd" d="M5.23 7.21a.75.75 0 011.06.02L10 11.168l3.71-3.938a.75.75 0 111.08 1.04l-4.25 4.5a.75.75 0 01-1.08 0l-4.25-4.5a.75.75 0 01.02-1.06z" clip-rule="evenodd" />
            </svg>
          </div>
          <input type="tel" name="phone-number" id="phone-number" v-model="PhoneNumber" autocomplete="tel" class="block w-full rounded-md border-0 px-3.5 py-2 pl-20 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
        </div>
      </div>

      <div class="flex gap-x-4 sm:col-span-2">
            <input id="acceptPub" v-model="acceptPub" name="acceptPub" type="checkbox" class="h-4 w-4 rounded border-gray-300 text-yellow-600 focus:ring-yellow-600">
        <label class="text-sm leading-6 text-gray-600" id="switch-1-label">
            En selectionnant cela vous acceptez de recevoir nos newsletters.
        </label>
      </div>
    </div>
    <div class="mt-10">
      <button type="submit" class="block w-full rounded-md bg-yellow-500 px-3.5 py-2.5 text-center text-sm font-semibold text-white shadow-sm hover:bg-yellow-600 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Reserver</button>
    </div>
  </form>
  <div v-if="reservationMessage" class="bg-green-200 text-green-800 rounded-md p-3 mt-4">
    {{ reservationMessage }}
  </div>

</div>
</template>

<style scoped>
@tailwind base;
@tailwind components;
@tailwind utilities;

.background-container {
  @apply bg-cover bg-center min-h-screen flex justify-center items-center;
  background-image: url('@/assets/background.png');
  background-position: left;
}

.glass-effect {
  @apply bg-opacity-50 shadow-md;
  background-color: rgba(255, 255, 255, 0.3); /* Couleur de fond transparente */
}
</style>

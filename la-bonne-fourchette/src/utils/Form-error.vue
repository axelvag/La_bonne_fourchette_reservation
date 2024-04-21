<script>
export const validateDateFormat = (date) => {
  // verifier le format YYYY-MM-DD
  const regex = /^\d{4}-\d{2}-\d{2}$/;
  return regex.test(date);
};

export const validateHeureFormat = (heure) => {
    const heureRegex = /^(0[0-9]|1[0-9]|2[0-3]):[0-5][0-9]$/;
  return heureRegex.test(heure);
};

export const  validateFirstNameFormat = (firstName) => {
    const regex = /\d/;
    return !regex.test(firstName);
}

</script>

<template>
</template>


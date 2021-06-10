<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="data"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </div>
</template>

<script>
export default {
  data: () => ({
    headers: [
      { text: "País", value: "country" },
      { text: "Ciudad", value: "city" },
      { text: "Nombre", value: "name" },
      { text: "Apellido", value: "lastName" },
      { text: "Email", value: "email" },
      { text: "Teléfono", value: "phoneNumber" },
    ],
    data: localStorage.data ? JSON.parse(localStorage.data) : [],
  }),

  mounted() {},
};
</script>
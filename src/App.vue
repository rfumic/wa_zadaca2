<template>
  <v-app>
    <v-card width="1000" class="mx-auto mt-10 px-5">
      <v-card-title>
        <h1 class="display-1 mx-auto">{{ unos ? unos : "Promjeni me" }}</h1>
      </v-card-title>
      <v-text-field
        v-model="unos"
        label="Ime"
        outlined
        append-outer-icon="mdi-send"
        @click:append-outer="getData"
        clearable
      ></v-text-field>

      <v-data-table :headers="headers" :items="podaci"></v-data-table>
    </v-card>
  </v-app>
</template>

<script>
export default {
  name: "App",

  computed: {
    headers() {
      return [
        { text: "Ime", value: "ime" },
        { text: "Drzave i Vjerojatnosti", value: "drzava" },
        { text: "Godine", value: "godine" },
        { text: "Spol i Vjerojatnost", value: "spol" },
      ];
    },
  },
  data: () => ({
    podaci: [],
    unos: "",
  }),
  methods: {
    async getData() {
      console.log("Pozvano");
      console.log(this.unos);
    },
  },
};
</script>

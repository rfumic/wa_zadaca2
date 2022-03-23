<template>
  <v-app>
    <v-card width="1000" class="mx-auto mt-10 px-5">
      <v-card-title>
        <h1 class="display-1 mx-auto">Prikaz podataka s 3 API endpointa</h1>
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
      // Headeri za tablicu
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
      try {
        // Dohvat Spola
        let response = await fetch(
          "https://api.genderize.io?name=" + this.unos
        );
        let result = await response.json();
        const spol = result.gender + " " + result.probability * 100 + "%";

        // Dohvat godina
        response = await fetch("https://api.agify.io?name=" + this.unos);
        result = await response.json();
        const godine = result.age;

        // Dohvat drzave
        response = await fetch("https://api.nationalize.io?name=" + this.unos);
        result = await response.json();
        const drzava = `${result.country[0].country_id} ${result.country[0].probability}, 
        ${result.country[1].country_id} ${result.country[1].probability},
        ${result.country[2].country_id} ${result.country[2].probability}`;

        // Unos u podaci array
        this.podaci.push({
          ime: this.unos,
          godine: godine,
          spol: spol,
          drzava: drzava,
        });
      } catch (err) {
        console.error(err);
      }
    },
  },
};
</script>

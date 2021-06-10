<template>
  <div>
    <v-form ref="form" v-model="validForm" lazy-validation>
      <v-row>
        <v-col offset-md="2" md="4">
          <v-select
            v-model="country"
            :items="countries"
            item-text="name"
            item-value="id"
            label="Seleccionar Pais"
            required
            return-object
          ></v-select>
        </v-col>

        <v-col md="4">
          <v-select
            v-model="city"
            :items="cities"
            item-text="name"
            item-value="id"
            label="Seleccionar Ciudad"
            required
            return-object
          ></v-select>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12" offset-md="2" md="4">
          <v-text-field
            v-model="name"
            label="Nombre"
            :rules="nameRules"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-text-field
            v-model="lastName"
            label="Apellido"
            required
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12" offset-md="2" md="4">
          <v-text-field
            v-model="email"
            label="Email"
            :rules="emailRules"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="4">
          <v-row>
            <v-col md="4">
              <v-select
                v-model="phoneCode"
                :items="phoneCodes"
                :rules="phoneCodeRules"
                item-text="dial_code"
                item-value="code"
                required
                return-object
              ></v-select>
            </v-col>

            <v-col>
              <v-text-field
                v-model="phoneNumber"
                :rules="phoneRules"
                label="Teléfono"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-col>
      </v-row>

      <v-row>
        <v-col offset-md="8" md="2">
          <v-btn color="success" @click="enviarFormulario">
            Enviar Formulario</v-btn
          >
        </v-col>
      </v-row>
    </v-form>
  </div>
</template>

<script>
import countries from "../data/countries/countries.json";
import phoneCode from "../data/countries/phoneCode.json";
import states from "../data/countries/states.json";
import cities from "../data/countries/cities.json";

export default {
  name: "Formulario",
  data: () => ({
    country: "",
    city: "",
    name: "",
    lastName: "",
    email: "",
    phoneNumber: "",
    countries: [],
    phoneCodes: [],
    phoneCode: "",
    nameRules: [
      (v) => !!v || "Nombre es requerido",
      (v) => v.length <= 40 || "Debe tener un máximo de 40 caracteres",
    ],
    emailRules: [
      (v) => !!v || "Email es requerido",
      (v) => /.+@.+/.test(v) || "Debe tener un formato válido",
      (v) => v.length <= 255 || "Debe tener un máximo de 255 caracteres",
    ],

    phoneCodeRules: [(v) => !!v || "Codigo es requerido"],

    validForm: true,
  }),

  methods: {
    enviarFormulario() {
      if (!this.$refs.form.validate()) return;

      let data = {
        country: this.country.name,
        city: this.city.name,
        name: this.name,
        lastName: this.lastName,
        email: this.email,
        phoneNumber: this.phoneCode.dial_code + this.phoneNumber,
      };

      let jsonList = localStorage.data ? JSON.parse(localStorage.data) : [];
      jsonList.push(data);

      localStorage.data = JSON.stringify(jsonList);

      this.limpiarCampos();
    },

    limpiarCampos() {
      this.name = "";
      this.lastName = "";
      this.country = "";
      this.city = "";
      this.email = "";
      this.phoneNumber = "";
      this.phoneCode = "";
      this.$refs.form.resetValidation();
    },

    getCitiesForCountry(country) {
      //Filtra las ciudades por el pais seleccionado
      if (!country) return [];

      let statesList = states.states;
      let filterState = statesList.filter(
        (state) => state.id_country == country
      );

      let citiesList = cities.cities;
      let data = [];
      filterState.forEach((state) => {
        data = data.concat(
          citiesList.filter((city) => city.id_state == state.id)
        );
      });

      return data;
    },

    validPhoneNumber(phoneCode) {
      //Validad del numero de telefono movil de los Paises España y Colombia
      if (!phoneCode) return;
      if (phoneCode.code == "ES") {
        return [
          (v) =>
            (/(6|7)([0-9]){8}/.test(v) && v.length <= 9) ||
            "Debe tener un formato válido",
        ];
      } else if (phoneCode.code == "CO") {
        return [
          (v) =>
            (/^3[\d]{9}$/.test(v) && v.length <= 10) ||
            "Debe tener un formato válido",
        ];
      }
    },
  },

  computed: {
    phoneRules() {
      return this.validPhoneNumber(this.phoneCode);
    },

    cities() {
      return this.getCitiesForCountry(this.country.id);
    },
  },

  mounted() {
    this.countries = countries.countries;
    this.phoneCodes = phoneCode.countries;
  },
};
</script>

<style scoped>
</style>

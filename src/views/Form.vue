<template>
  <div>
    <div class="text-h5 text-center mt-12 mb-5">Formulario para carga de datos</div>
    <Forms />
    <v-row>
      <v-col offset-md="2">
        <v-btn color="primary" @click="showDialog = true">Ver datos registrados</v-btn>
      </v-col>
    </v-row>
    <v-dialog
      v-if="showDialog"
      v-model="showDialog"
      transition="dialog-top-transition"
      max-width="800"
      persistent
    >
      <v-card>
        <v-toolbar color="primary" dark>Datos registrados</v-toolbar>
        <br />
        <v-card-text>
          <TableForm />
        </v-card-text>
        <v-card-actions class="justify-end">
          <v-btn class="primary" @click="showDialog = false">Cerrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import Forms from "../components/Forms.vue";
import TableForm from "../components/TableForm.vue";

export default {
  data: () => ({
    showDialog: false,
  }),

  methods: {},

  components: {
    Forms,
    TableForm,
  },
};
</script>
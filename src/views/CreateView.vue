<template>
  <div>
    <formFichaMedica
      @enviarData="enviarData"
      :formData="formData"
      titulo="Crear Elemento"
      :limpiar="true"
    >
      <router-link :to="'/'">
        <v-btn class="mx-2" fab dark large color="cyan">
          <v-icon dark> mdi-keyboard-return </v-icon>
        </v-btn>
      </router-link></formFichaMedica
    >
  </div>
</template>

<script>
import FullBoxVue from "../components/static/FullBox.vue";
import formFichaMedica from "@/components/formFichaMedica.vue";
import { crear } from "../api";

export default {
  name: "HomeView",
  components: {
    FullBoxVue,
    formFichaMedica,
  },
  data: () => ({
    formData: {
      name: "",
      price: "",
      id: "",
    },
  }),
  methods: {
    enviarData: function () {
      console.log(this.formData);
      crear(this.formData).then((response) => {
        this.$router.push("/")
      });
    },
    reset: function () {
      console.log("Limpiando");
    },
  },
};
</script>

<style scoped>
.home {
  background: #fff;
}

.centerDiv {
  margin: 0 auto;
}
</style>

<template>
    <formFichaMedica
      @enviarData="enviarData"
      :formData="formData"
      titulo="Actualizar item"
      :limpiar="false"
    >
      <router-link :to="'/'">
        <v-btn class="mx-2" fab dark large color="cyan">
          <v-icon dark> mdi-keyboard-return </v-icon>
        </v-btn>
      </router-link>
    </formFichaMedica>
</template>

<script>
import FullBoxVue from "../components/static/FullBox.vue";
import formFichaMedica from "@/components/formFichaMedica.vue";
import { listarId, crear } from "../api";

export default {
  name: "HomeView",
  components: {
    FullBoxVue,
    formFichaMedica,
  },
  data() {
    return {
      idSolicitud: this.$route.params.id,
      formData: {
        id: this.$route.params.id,
        name: "",
        price: "",
      },
    };
  },
  methods: {
    getData: function () {
      listarId(this.idSolicitud).then((response) => {
        this.formData = response.data[0];
      });
    },
    enviarData: function () {
      crear(this.formData).then((response) => {});
    },
  },
  mounted() {
    this.getData();
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

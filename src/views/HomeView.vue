<template>
  <MiddleBox class="shadowHover centerDiv">

    <h1>Registros de items</h1>
    <router-link :to="'/crear'">
      <v-btn class="mx-2" fab dark large color="cyan">
        <v-icon dark> mdi-plus </v-icon>
      </v-btn>
    </router-link>


    <v-data-table
      :headers="headers"
      :items="dataResponse"
      :search="search"
      :items-per-page="30"
      class="elevation-1"
    >
      <template v-slot:item.actions="{ item }">
        <v-btn class="mx-2" fab dark small color="cyan" @click="editItem(item)"
          ><v-icon small> mdi-pencil </v-icon></v-btn
        >
        <v-btn
          class="mx-2"
          fab
          dark
          small
          color="error"
          @click="deleteItem(item)"
          ><v-icon small> mdi-delete </v-icon></v-btn
        >
      </template>
    </v-data-table>
    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card>
        <v-card-title class="text-h6" style="text-align: center"
          >¿Estás seguro/a de eliminar este item?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
          <v-btn color="blue darken-1" text @click="deleteItemConfirm"
            >OK</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </MiddleBox>
</template>

<script>
import MiddleBox from "../components/static/MiddleBox.vue";
import { listar, eliminar } from "../api";

export default {
  name: "HomeView",
  components: {
    MiddleBox,
  },
  data: () => ({
    headers: [
      { text: "Id", align: "start", value: "id" },
      { text: "Precio", value: "price" },
      { text: "Nombre", value: "name" },
      { text: "Acciones", value: "actions", sortable: false },
    ],
    dataResponse: [],
    search: "",
    idSelected: 0,
    dialogDelete: false,
  }),
  methods: {
    cleanDataResponse: function(data){
      let arrayReturn = []
      for(let i in data){
        arrayReturn.push(data[i][0])
      }
      return arrayReturn
    },
    getData: function () {

      listar().then((response) => {
        this.dataResponse = this.cleanDataResponse(response.data);
      });
    },
    editItem: function (item) {
      this.$router
        .push({ path: "/editar/" + item.id })
        .catch(() => {});
    },
    deleteItem: function (item) {
      this.idSelected = item.id;
      this.dialogDelete = true;
    },

    deleteItemConfirm: function () {
      eliminar(this.idSelected).then((response) => {this.getData();});

      this.dialogDelete = false;

    },

    closeDelete: function () {
      this.dialogDelete = false;
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
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>

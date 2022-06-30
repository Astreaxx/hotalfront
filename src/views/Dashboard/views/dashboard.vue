<template>
<div>
 <container>
    <v-row>

        <v-col  md="3">
            <!--Card 1-->
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content  >
                    <v-list-item-title class="text-h6 mb-1">
                        <h3> {{ habitacionesVisualizar }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones disponibles </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="green"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="3">
            <!--Card 2-->
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1" >
                        <h3> {{ habitacionesVisualizar1 }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones ocupadas </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="red"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="3">
            <!--Card 3-->
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                        <h3> {{ habitacionesVisualizar2 }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones reservadas </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="yellow"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>     
           
        <v-col  md="3">
            <!--Card 4-->
            <v-card
                class="mx-auto"
                max-width="280"
                shaped
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1"> 
                        <h3> {{ habitacionesVisualizar3 }} </h3>
                    </v-list-item-title>
                    <v-list-item-subtitle> Habitaciones en limpieza </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="blue"
                size="50px"
                >
                    mdi-bed  
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>
       

        <v-col  md="4">
            <!--Card 5-->
            <v-card
                class="mx-auto"
                max-width="280"
                outline
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                    habitacion
                    </v-list-item-title>
                    <v-list-item-subtitle> Reservas</v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="gray"
                size="50px"
                >
                    mdi-bell 
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="4">
            <!--Card 6-->
            <v-card
                class="mx-auto"
                max-width="280"
                outline
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                    habitacion
                    </v-list-item-title>
                    <v-list-item-subtitle> Check-In </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="gray"
                size="50px"
                >
                    mdi-bell
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

        <v-col  md="4">
            <!--Card 7-->
            <v-card
                class="mx-auto"
                max-width="280"
                outline
            >
                <v-list-item two-line>
                <v-list-item-content>
                    <v-list-item-title class="text-h6 mb-1">
                    habitacion
                    </v-list-item-title>
                    <v-list-item-subtitle> Check-Out </v-list-item-subtitle>
                </v-list-item-content>
                <v-icon
                color="gray"
                size="50px"
                >
                    mdi-bell 
                </v-icon>
                </v-list-item>
            </v-card>
        </v-col>

    </v-row>
 </container>
</div>
   
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
      
    reservaVisualizar: [], 
    habitacionesVisualizar: [],
    habitacionesVisualizar1: [],
    habitacionesVisualizar2: [],
    habitacionesVisualizar3: [],

    estado: [
      {
        nombre: "Reservadas"
      },
      {
        nombre: "Limpieza"
      },
      {
        nombre: "Ocupada"
      },
      {
        nombre: "Disponible"
      }
    ],
    
  }),

  created() {
    this.obtenerHabitaciones();
    this.obtenerHabitaciones1();
    this.obtenerHabitaciones2();
    this.obtenerHabitaciones3();
  },

  methods: {

    obtenerHabitaciones: async function () {
      await axios.get("http://localhost:3000/habitaciones/").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerHabitaciones1: async function () {
      await axios.get("http://localhost:3000/habitaciones/list1").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar1 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerHabitaciones2: async function () {
      await axios.get("http://localhost:3000/habitaciones/list2").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar2 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

    obtenerHabitaciones3: async function () {
      await axios.get("http://localhost:3000/habitaciones/list3").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar3 = resp.data.length;
          console.log('------------------------------------------');
          console.log(resp.data.length);

        }
      });
    },

  }

};
</script>

<style scoped>
    #text{
        text-align: center;
        margin-top: 13%;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        font-size: 2.2rem;
    }
</style>
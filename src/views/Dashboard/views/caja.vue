<template>
  <v-container>
    <v-row justify="center">
     <v-dialog v-model="dialogCrear" persistent max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" dark v-bind="attrs" v-on="on"> crear caja </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">Asignacion de datos ciudad</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-form v-model="validacionCreacion">
                <v-row no-gutters>
                  <v-card class="mx-auto" max-width="700" outlined id="margen">
                    <center>
                      <h2 id="margen">Registro de Caja</h2>
                    </center>
                    <v-form ref="form" v-model="valid" lazy-validation id="margen">
                      <v-col cols="12" sm="6">
                       <v-col cols="12" sm="6">
                       <v-select
                          v-model="cajaa.idusuario"
                          :items="ObtenerCaja"
                          item-text="nombre_caja"
                          item-value="nombre_caja"
                          label="Usuario"
                          :rules="Rules"
                          required
                        ></v-select>
                      </v-col>
                      </v-col>

                      <v-container>
                        <v-row justify="center">
                          <v-col md="5" style="margin-right: 30px">
                            <label>nombre</label>
                            <v-text-field
                              v-model="cajaa.nombre_caja"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Descripcion</label>
                            <v-text-field
                              v-model="cajaa.descripcion"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Fecha APertura</label>
                            <v-text-field
                              v-model="cajaa.fecha_apertura"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad INicial</label>
                            <v-text-field
                              v-model="cajaa.cantidad_inicial"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad CIERRE</label>
                            <v-text-field
                              v-model="cajaa.cantidad_cierre"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad </label>
                            <v-text-field v-model="cajaa.monto" :rules="Rules" required>
                            </v-text-field>
                          </v-col>

                          <v-col md="5" style="margin-right: 30px">
                            <label>Ubicacion</label>
                            <v-text-field
                              v-model="cajaa.ubicacion"
                              :rules="Rules"
                              required
                            >
                            </v-text-field>
                          </v-col>

                          <v-btn
                            :disabled="!valid"
                            class="mr-4"
                            color="success"
                            @click="caja()"
                          >
                            Crear Caja
                          </v-btn>
                        </v-row>
                      </v-container>
                    </v-form>
                  </v-card>
                </v-row>
              </v-form>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialogCrear = false">
              Cerrar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-col md="12">
        <v-col md="6" id="text-title"><h3>Aperturas de Caja</h3></v-col>
        <v-simple-table height="600px">
          <template v-slot:default>

            <thead>
            <tr>
              <th class="text-left">Nombre caja </th>
              <th class="text-left">Descripcion</th>
              <th class="text-left">Fecha Apertura</th>
              <th class="text-left">Cantidad Inicial</th>
              <th class="text-left">Cantidad Cierre</th>
              <th class="text-left">Cantidad</th>
              <th class="text-left">Ubicacion</th>
              <th class="text-left"></th>
            </tr>
            </thead>
            <tbody>

            <tr v-for="(item, i) in ObtenerCaja" :key="i">
              <td>{{ item.nombre_caja }}</td>
              <td>{{ item.descripcion }}</td>
              <td>{{ item.fecha_apertura }}</td>
              <td>{{ item.cantidad_inicial }}</td>
              <td>{{ item.cantidad_cierre }}</td>
              <td>{{ item.monto }}</td>
              <td>{{ item.ubicacion }}</td>

            </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>
  </v-container>
</template>



<script>
import axios from 'axios'
export default {
  data: () => ({
    Rules: [(v) => !!v || "El campo es requerido"],
    ObtenerUsuarios: [],
    ObtenerCaja:[],
    cajaa:{
      idusuario:"",
      nombre_caja:"",
      descripcion:"",
      fecha_apertura:"",
      cantidad_inicial:0,
      cantidad_cierre:0,
      monto:0,
      ubicacion:"",
      estado:1,

    }
  }),
    created() {
      this.obtenerUsuarios();
      this.obtenerCaja();
       this.filtrar();
    },

    methods: {
       filtrar(){
      this.trabajador = parseInt(window.sessionStorage.getItem("seleccionadoAdmin"))      
      this.obtenerRoles();
    },
     obtenerRoles: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/rol").then((resp) => {
            if (resp.status == 200) {
              for (let index = 0; index < resp.data.length; index++) {
                const element = resp.data[index];
                 
                if (element.idrRol == this.trabajador) {
                  this.ObtenerRoles = element;
                }
              }
              console.log("Yo soy el rol: ", this.ObtenerRoles);
             
            
            }
        });
      },




      caja: async function(){  
      let send ={
        idusuario:this.cajaa.idusuario,
        nombre_caja:this.cajaa.nombre_caja,
        descripcion:this.cajaa.descripcion,
        fecha_apertura:this.cajaa.fecha_apertura,
        cantidad_inicial:this.cajaa.cantidad_inicial,
        cantidad_cierre:this.cajaa.cantidad_cierre,
        monto:this.cajaa.monto,
        ubicacion:this.cajaa.ubicacion,
        estado:1
      }
      await  axios.post("http://localhost:3000/caja/create",send).then((resp) => {
        if (resp.status == 201) {

          alert("Caja creada Correctamente");
          location.reload();

        }
      })

    },
      obtenerCaja: async  function(){
        await  axios.get('http://localhost:3000/caja').then((resp)=>{


          if (resp.status == 200){

            this.ObtenerCaja= resp.data
            console.log(this.obtenerCaja);
          }
        })

      },

      obtenerUsuarios: async function () {

        await axios.get("http://localhost:3000/usuario").then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuarios = resp.data;
          }
        });

      },
    },
};
</script>


<style scoped>
#margen {
  margin-top: 30px;
}
</style>

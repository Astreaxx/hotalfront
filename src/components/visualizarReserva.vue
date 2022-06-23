<template>
  <v-container>
    <v-row>
        <v-col md="6" id="text-title"><h3>Reservas hechas</h3></v-col>
      <v-col md="12">
          <v-simple-table height="300px">
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Usuario</th>
              <th class="text-left">Tipo habitación</th>
              <th class="text-left">Habitación</th>
              <th class="text-left">Servicio</th>
              <th class="text-left">Catidad Personas</th>
              <th class="text-left">Fecha inicio</th>
              <th class="text-left">Fecha fin</th>
              <th class="text-left">Estado</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, i) in reservaVisualizar" :key="i">
              <td>
                <div v-for="(itemUser, i) in usuariosVisualizar" :key="i">
                  <div v-if="item.idusuario == itemUser.idUsuario">
                    {{itemUser.nombre}}
                  </div>
                </div>
              </td>
              <td>
                <div v-for="(itemCate, i) in categoriaHabitacion" :key="i">
                  <div v-if="item.idcategoriaHab == itemCate.idcategoriaHab">
                    {{itemCate.titulo}}
                  </div>
                </div>
              </td>
              <td>
                <div v-for="(itemHab, i) in habitacionesVisualizar" :key="i">
                  <div v-if="item.idhabitacion == itemHab.idhabitacion">
                    {{itemHab.nombre}}
                  </div>
                </div>
              </td>
              <td>
                <div v-for="(itemServ, i) in Servicioshabitacion" :key="i">
                  <div v-if="item.idservicio == itemServ.idservicio">
                    {{itemServ.servicio}}
                  </div>
                </div>
              </td>
              <th>{{item.cantPersonas}}</th>
              <td>{{item.fechaEntrada}}</td>
              <td>{{item.fechaSalida}}</td>
              <td>
              
                  <div v-if="item.estado == 0">Eliminada</div>
                  <div v-if="item.estado == 1">Espera</div>
                  <div v-if="item.estado == 2">Rechazada</div>
                  <div v-if="item.estado == 3">probada</div>
                  <div v-if="item.estado == 4">Activa</div>
                  <div v-if="item.estado == 5">Finalizada</div>
              </td>
              <td>
              
               
                    <div class="text-center">
                      <v-dialog v-model="dialog" width="600">
                        <template v-slot:activator="{ on, attrs }">
                          <v-btn 
                          v-if="item.estado == 1"
                          class="mr-4" 
                          color="success"
                          v-bind="attrs" 
                          v-on="on" 
                          fab
                          dark
                         small
                         
                          >
                          <v-icon dark>
                              mdi-account-minus
                          </v-icon>
                          </v-btn>
                        </template>

                        <v-card>
                          <v-card-title class="text-h5 blue-grey darken-2">
                            Cancelar la reserva
                          </v-card-title>
                          <v-card-text style="text-align: justufy;">
                            Estoy consciente que al hacer la cancelación de mi reserva 5 dás antes de la fecha estipulada 
                            acpto una retención del 20% del monto pago.
                            <v-checkbox style="margin-top: 20px" v-model="checkbox1"
                              label="Acepto las condiciones de uso"></v-checkbox>
                          </v-card-text>

                          <v-divider></v-divider>

                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" text :disabled="!checkbox1" @click="EliminarReservas(item.idreserva), dialog = false">
                              Cancelar
                            </v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                    </div>
                    <div class="text-center">
                      <v-dialog v-model="dialog2" width="600">
                        <template v-slot:activator="{ on, attrs }">
                          <v-btn 
                          v-if="item.estado == 0"
                          class="mr-4" 
                          color="success"
                          v-bind="attrs" 
                          v-on="on" 
                          fab
                          dark
                         small
                         
                          >
                          <v-icon dark>
                              mdi-account-minus
                          </v-icon>
                          </v-btn>
                        </template>

                        <v-card>
                          <v-card-title class="text-h5 blue-grey darken-2">
                            Condiciones para la reservación la reservación
                          </v-card-title>
                          <v-card-text>
                            Al hacer click en el boton de abajo acepto que revise el aviso de privacidad,
                            las aletas de viajes del gobierno, tambien acepto las normas y restricciones
                            <v-checkbox style="margin-top: 20px" v-model="checkbox2"
                              label="Acepto las condiciones de uso"></v-checkbox>
                          </v-card-text>

                          <v-divider></v-divider>

                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="primary" text :disabled="!checkbox2"   @click="RecuperarReservas(item.idreserva), dialog = false">
                              recuperar
                            </v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                    </div>
              </td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
      </v-col>

         <v-row justify="start">
                  <v-col cols="12" sm="6">
                      <v-select
                        
                        v-model="serviciocliente.idservicio"
                        :items="Servicioshabitacion"
                        item-text="servicio"
                        item-value="idservicio"
                        label="Servicio"
                        :rules="Rules"
                        required
                      ></v-select>
                    
                    </v-col>
                     <v-col cols="12" sm="6">
                      <v-text-field
                        v-model="serviciocliente.cantidad"
                        type="number"
                        label="Cantidad"
                        :rules="Rules"
                        required
                      ></v-text-field>
                    </v-col>
                    
                    <v-btn text @click="serviciocliente()"> añadir </v-btn>
                   
         
         </v-row>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
      reservaVisualizar: [],
      habitacionesVisualizar: [],
      categoriaHabitacion: [],
      Servicioshabitacion: [],
      usuariosVisualizar: [],
      serviciocliente:{

        idservicio:1,
        cantidad: 1,

      },
      dialog: false,
      dialog2: false,
      checkbox1: false,
      checkbox2: false,
      
  }),

  created(){
      this.obtenerReservas();
      this.serviciocliente();
  },

  methods:{

     serviciocliente: async function () {
        let send = {
        idrservicio: this.serviciocliente.idrservicio,
        cantidad: parseInt(this.serviciocliente.cantidad),
     
      }
      console.log("Yo soy registrar usuario", send);
      console.log(this.serviciocliente);
      await axios
        .post("http://localhost:3000/serviciocliente/create", this.serviciocliente)
        .then((resp) => {
                if (resp.status == 201) {
                   this.respuesta = resp.data ;
                   this.obtenerReservas;
                   console.log("entidad id",this.respuesta);  
                }

            })
            .catch(error => {
                let respu = error.message
                alert(respu);
                console.log(this.respuesta);
            })
        },
      obtenerReservas: async function () {
          let usuario = parseInt(window.sessionStorage.getItem("seleccionado"));
        console.log("Hola");
        await axios.get("http://localhost:3000/reserva/"+usuario).then((resp) => {
            if (resp.status == 200) {
            this.reservaVisualizar = resp.data;
            this.obtenerUsuarios();
            this.obtenerTipoHabitaciones();
            this.obtenerHabitaciones();
            this.obtenerServicios();
            }
        });
        console.log(this.reservaVisualizar);
      },

      obtenerUsuarios: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/usuario").then((resp) => {
            if (resp.status == 200) {
            this.usuariosVisualizar = resp.data;
            }
        });
      },

      obtenerTipoHabitaciones: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/categoriaHab").then((resp) => {
            if (resp.status == 200) {
            this.categoriaHabitacion = resp.data;
            }
        });
        console.log(this.categoriaHabitacion);
      },

      obtenerHabitaciones: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/habitaciones/obtener").then((resp) => {
            if (resp.status == 200) {
            this.habitacionesVisualizar = resp.data;
            }
        });
        console.log(this.habitacionesVisualizar);
      },

      obtenerServicios: async function () {
        console.log("Hola");
        await axios.get("http://localhost:3000/servicios").then((resp) => {
            if (resp.status == 200) {
            this.Servicioshabitacion = resp.data;
            }
        });
        console.log(this.Servicioshabitacion);
      },
         RecuperarReservas: async function (id) {
      let Aidi = parseInt(id)
      let estado = { estado: 1 };

      await axios.put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado).then((resp) => {
        if (resp.status == 204) {
          this.obtenerReservas();
        }
         alert("Reserva restaurada correctamente.");
            location.replace("/visualizar");
      });
      console.log(this.ObtenerEntidades);
    },
         EliminarReservas: async function (id) {
      let Aidi = parseInt(id)
      let estado = { estado: 0 };

      await axios.put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado).then((resp) => {
        if (resp.status == 204) {
          this.obtenerReservas();
        }
         alert("Reserva cancelada correctamente.");
            location.replace("/visualizar");
      });
      console.log(this.ObtenerEntidades);
        
    },
  }
};
</script>


<style scoped>
    #text-title{
        margin-top: 20px;
        margin-left: 40%;
    }
</style>
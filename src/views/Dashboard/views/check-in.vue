<template>
  <v-container>
    <h2 id="text-title">Caja actual</h2>

      <v-col md="12">
        <v-col id="text-title"><h3>Aperturas de Caja</h3></v-col>
        <v-simple-table height="300px" width="20px" style="text-align: center;">
          <template v-slot:default>
            <thead>
            <tr>
              <th style="text-align: center;" >Nombre caja </th>              
              <th style="text-align: center;">Solicitar</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(item, i) in ObtenerCaja" :key="i">
              <td>{{ item.nombre_caja }}</td>
              <td>

       <v-dialog v-model="dialogCrear2" persistent max-width="600px">
        <template v-slot:activator="{ on, attrs }">
          <v-btn  @click="stateRolsUsers3(item.idCaja)" color="primary" dark v-bind="attrs" v-on="on"> Solicitar caja </v-btn>
        </template>
        <v-card>
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
                        <label>Nombre de usuario<h3 id="a">{{cajaa2.idusuario}}</h3><hr></label>                      
                        
                      </v-col>
                      <v-container>
                        <v-row justify="center">
                          <v-col md="5" style="margin-right: 30px">
                            <label>Nombre caja<h3 id="a">{{cajaa2.nombre_caja}}</h3><hr></label>
                            
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Descripcion</label>
                            <v-text-field
                              v-model="cajaa2.descripcion"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Fecha apertura</label>
                            <v-text-field
                              v-model="cajaa2.fecha_apertura"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad inicial</label>
                            <v-text-field
                              v-model="cajaa2.cantidad_inicial"
                              :rules="Rules"
                              required
                            ></v-text-field>
                          </v-col>
                          <v-col md="5" style="margin-right: 30px">
                            <label>Cantidad </label>
                            <v-text-field v-model="cajaa2.monto" :rules="Rules" required>
                            </v-text-field>
                          </v-col>
                          <v-btn
                            :disabled="!valid"
                            class="mr-4"
                            color="success"
                            @click="cambiarRol(item.idCaja,cajaa2)"
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
            <v-btn color="blue darken-1" text @click="dialogCrear2 = false">
              Cerrar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
     </td>

            </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
<v-row>
      <v-col id="text-title">
        <h3>Check-In</h3>
      </v-col>
      <v-col md="12">
        <v-simple-table height="200px">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Usuario</th>
                <th class="text-left">Tipo habitación</th>
                <th class="text-left">Habitación</th>
                <th class="text-left">Servicio</th>
                <th class="text-left">Catidad Personas</th>
                <th class="text-left">Estado</th>
                <th class="text-left">Fecha de creación</th>
                <th class="text-left">Fecha inicio</th>
                <th class="text-left">Fecha fin</th>
                <th class="text-left">Añadir acompañante</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="item.estado == 3" v-for="(item, i) in reservaVisualizar" :key="i">
                <td>
                  <div v-for="(itemUser, i) in usuariosVisualizar" :key="i">
                    <div v-if="item.idusuario == itemUser.idUsuario">
                      {{ itemUser.nombre }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemCate, i) in categoriaHabitacion" :key="i">
                    <div v-if="item.idcategoriaHab == itemCate.idcategoriaHab">
                      {{ itemCate.titulo }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemHab, i) in habitacionesVisualizar" :key="i">
                    <div v-if="item.idhabitacion == itemHab.idhabitacion">
                      {{ itemHab.nombre }}
                    </div>
                  </div>
                </td>
                <td>
                  <div v-for="(itemServ, i) in Servicioshabitacion" :key="i">
                    <div v-if="item.idservicio == itemServ.idservicio">
                      {{ itemServ.servicio }}
                    </div>
                  </div>
                </td>
                <th>{{ item.cantPersonas }}</th>
                <td>
                  <div v-if="item.estado == 1">Espera</div>
                  <div v-if="item.estado == 2">Rechazada</div>
                  <div v-if="item.estado == 3">probada</div>
                  <div v-if="item.estado == 4">Activa</div>
                  <div v-if="item.estado == 5">Finalizada</div>
                </td>
                <td>{{ item.created_at.substr(2, 8) }}</td>
                <td>{{ item.fechaEntrada }}</td>
                <td>{{ item.fechaSalida }}</td>

                <td>
                  <v-row justify="start">
                    <v-dialog v-model="dialogCrear" persistent max-width="600px">
                      <template v-slot:activator="{ on, attrs }">
                        <v-btn
                          class="mx-2"
                          v-bind="attrs"
                          v-on="on"
                          v-if="item.estado == 3"
                          @click="BorrarReservas(item.idreserva) && CambioEstado(item.idhabitacion, EstadoSeleccionado) && stateRolsUsers2(item.idreserva)"
                          fab
                          dark
                          small
                          color="blue"
                        >
                          <v-icon dark> mdi-plus </v-icon>
                        </v-btn>
                        <!-- a -->

                        <v-btn
                          class="mx-2"
                          v-if="item.estado == false"
                          @click="RecuperarReservas(item.idreserva)"
                          fab
                          dark
                          small
                          color="yellow"
                        >
                          <v-icon dark> mdi-minus-circle </v-icon>
                        </v-btn>
                      </template>

                      <v-card>
                        <v-card-title>
                          <span class="text-h5">Registro de acompanante</span>
                        </v-card-title>
                        <v-card-text>
                          <v-container>
                            <v-form v-model="validacionCreacion">
                              <v-row no-gutters>
                                                 <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Nombre"
                                    type="text"
                                    label="Nombre completo"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>                             

                                <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Identificacion"
                                    type="text"
                                    label="Identificacion"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Telefono"
                                    type="text"
                                    label="Telefono"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col cols="12" sm="6">
                                  <v-text-field
                                    v-model="RegistrarAcompas.Edad"
                                    type="number"
                                    label="Edad"
                                    :rules="Rules"
                                    required
                                  ></v-text-field>
                                </v-col>

                                <v-col md="3" style="margin-right: 390px">
                                  <v-btn
                                    :disabled="valid"
                                    class="mr-4"
                                    color="success"
                                    @click="registrarAcompas(RegistrarAcompas)"
                                  >
                                    Enviar
                                  </v-btn>
                                </v-col>
                              </v-row>
                            </v-form>
                          </v-container>
                        </v-card-text>
                        <v-card-actions>
                          <v-spacer></v-spacer>
                          <v-btn color="blue darken-1" text @click="dialogCrear = false && cerrar()">
                            Cerrar
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </v-row>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>

      <!-- dialog de Creacion  de reservas -->

      <!-- Fin del dialog -->
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
    ObtenerUsuarios: [],
    ObtenerCaja: [],
    ObtenerUsuarios: [],
    habitacionesVisualizar: [],
    categoriaHabitacion: [],
    equipamientoVisualizar: [],
    ciudad: [],
    tipoentidad: [],
    actividadjurid: [],
    dialogCrear: false,
    dialogCrear2: false,

    validacionCreacion: true,
    menu: false,
    menu2: false,
    Rules: [(v) => !!v || "El campo es requerido"],
    reservar: {
      fechaEntrada: "",
      fechaSalida: "",
      cantPersonas: 0,
      estado: 0,
      idservicio: 0,
      idhabitacion: 0,
      idcategoriaHab: 0,
      idusuario: 0,
    },
    
    cajaa: {
      idusuario: "",
      nombre_caja: "",
      descripcion: "",
      fecha_apertura: "",
      cantidad_inicial: 0,
      cantidad_cierre: 0,
      monto: 0,
      ubicacion: "",
      estado: 1,
    },
      cajaa2:{
      idusuario:"",
      nombre_caja:"",
      descripcion:"",
      fecha_apertura:"",
      cantidad_inicial:0,
      cantidad_cierre:0,
      monto:0,
      ubicacion:"",
      estado:1,
      idrol:0,
      trabajador:0
    },
    RegistrarAcompas: {
      Nombre: "",
      Edad: 0,
      Identificacion: "",
      Telefono: "",
      idreserva: 0,
    },
    
   
    Rules: [(v) => !!v || "Campo requerido"],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+/.test(v) || "E-mail must be valid",
    ],

    natural: {
      primerNombre: "",
      segundoNombre: null,
      primerApellido: null,
      segundoApellido: "",
      fechaNacimiento: null,
      genero: "",
      telefono: " ",
      celular: " ",
      correo: "acompañanteeeeee@gmail.com",
      direccion: "mi casa",
      identificacion: "",
      tipoIdentificacion: 1,
      razonSocial: null,
      siglas: null,
      ruc: null,
      codigoPostal: null,
      idPais: 1,
      idCiudad: 1,
      idActividad: null,
      idTipoPersona: 1,
    },

    
    paisId: 0,
    paises: [],
    genero: [{ genero: "M" }, { genero: "F" }],
    tipoIdent: [{ tipo: "Cedula" }, { tipo: "Pasaporte" }],
    password: "",
    confirmPassword: "",
    respuesta: {},
    respuesta2: "",
    token: "",
    usuario: "",
    firstname: "",
    lastname: "",
    obtener: 0,
    idTipoPersona: 0,
    valid: false,   
    drawer: false,
    group: null,
    menu: false,
    modal: false,
    menu2: false,
    show1: false,
    show2: false,
    trabajador: 0,
    EstadoSeleccionado: "",
    buscar: "",
    estado: "Ocupada",
    idrol: 0,
    trabajador:0
  }),

  created() {
    this.obtenerReservas();
    this.obtenerHabitaciones();
    this.Pais();
    this.Ciudad();
    this.TipoEntidad();
    this.ActividadJuridica();
    this.obtenerUsuarios();
    this.obtenerCaja();
  },

  methods: {

     stateRolsUsers3: async function (id) {
      this.idrol = parseInt(id);
      console.log("Id rol: ", this.idrol);
      this.obtenerRoles();     
    },

     obtenerRoles: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/caja").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            if (element.idCaja == this.idrol) {
              this.cajaa2 = element;             
            }
          }
          console.log("Yo soy el rol: ", this.cajaa2);
          this.obtenerUsuario();
        }
      });
    },
    cambiarRol: async function (id) {
      let Aidi = parseInt(id);
      let estado = this.cajaa2;
      console.log("Id Reserva: ", Aidi, "Estado: ", estado);
      await axios
        .put("http://localhost:3000/caja/update/" + this.idrol, estado)
        .then((resp) => {
          if (resp.status == 204) {
            location.reload();
          }
        });
    },
    obtenerUsuario: async function () {
        this.trabajador = parseInt(window.sessionStorage.getItem("seleccionadoAdmin"))
        await axios.get("http://localhost:3000/usuario"+this.trabajador).then((resp) => {
          if (resp.status == 200) {
            this.ObtenerUsuario = resp.data;
          }
        });

      },
    cerrar: async function () {
         location.reload("/panel/check-in");
        },
     stateRolsUsers2: async function (id) {
      this.idrol = parseInt(id);
      this.RegistrarAcompas.idreserva= this.idrol;
      console.log("Id rol: ", this.idrol);   
    },
    registrarAcompas: async function () {
      // let Aidi = parseInt(id);
      console.log("Hola");
      console.log("acompa",this.RegistrarAcompas);
      let acompa = this.RegistrarAcompas;
      
      await axios.post("http://localhost:3000/acompanante/create", acompa)
        .then((resp) => {
           console.log("acompa",this.RegistrarAcompas)
          console.log("acompa",resp)
          if (resp.status == 201) {
            this.RegistrarAcompas = resp.data;
            console.log("entidad id", this.RegistrarAcompas);
            alert("acompañante agregado exitosamente");
            
          }
         
        })
        .catch((error) => {
          let respu = error.message;
          alert(respu);
          console.log(this.respuesta);
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
        await  axios.get('http://localhost:3000/caja')
        .then((resp)=>{
          console.log("CAJA",resp);
          if (resp.status == 200){
            this.ObtenerCaja= resp.data
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
    

    // otra vez vamos a ver xd

    EntidadNatural: async function () {
      console.log(this.natural);
      console.log(this.password);
      await axios
        .post("http://localhost:3000/entidad/create/", this.natural)
        .then((resp) => {
          if (resp.status == 201) {
            this.respuesta = resp.data;
            console.log("entidad id", this.respuesta);
            this.Usuario();
          }
        })
        .catch((error) => {
          let respu = error.message;
          alert(respu);
          console.log(this.respuesta);
        });
    },
    Pais: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/pais").then((resp) => {
        if (resp.status == 200) {
          for (let index = 0; index < resp.data.length; index++) {
            const element = resp.data[index];
            this.paises.push({
              idPais: element.idPais,
              Nombre: element.Nombre,
            });
          }
        }
      });
      console.log(this.paises);
    },
    Ciudad: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/ciudad").then((resp) => {
        if (resp.status == 200) {
          this.ciudad = resp.data;
        }
      });
      console.log(this.ciudad);
    },
    TipoEntidad: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/tipoentidad").then((resp) => {
        if (resp.status == 200) {
          this.tipoentidad = resp.data;
        }
      });
      console.log(this.tipoentidad);
    },
    ActividadJuridica: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/actividadJuridica").then((resp) => {
        if (resp.status == 200) {
          this.actividadjurid = resp.data;
        }
      });
      console.log(this.actividadjurid);
    },

    // vanmos a ver si sirve xd
    CambioEstado: async function (id, estadoCambio) {
      let Aidi = parseInt(id);
      let idreserva = this.idreserva;
      let estado = { estado: "Ocupada" };

      await axios
        .put(
          "http://localhost:3000/habitaciones/cambiarEstado/" + Aidi,
          estado,
          +idreserva
        )
        .then((resp) => {
          if (resp.status == 204) {
            this.obtenerHabitaciones();
            this.obtenerReservas();
          }
        });
    },
    obtenerTipoHabitaciones: async function () {
      await axios.get("http://localhost:3000/categoriaHab").then((resp) => {
        if (resp.status == 200) {
          this.categoriaHabitacion = resp.data;
        }
      });
    },

    obtenerHabitaciones: async function () {
      await axios.get("http://localhost:3000/habitaciones/obtener").then((resp) => {
        if (resp.status == 200) {
          this.habitacionesVisualizar = resp.data;
          this.obtenerTipoHabitaciones();
          this.obtenerEquipamiento();
        }
      });
    },

    obtenerEquipamiento: async function () {
      await axios.get("http://localhost:3000/equipamiento").then((resp) => {
        if (resp.status == 200) {
          this.equipamientoVisualizar = resp.data;
        }
      });
    },
    // otra cosaxd
    BorrarReservas: async function (id) {
      let Aidi = parseInt(id);
      let estado = { estado: 4 };
      console.log("Hola");
      await axios
        .put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado)
        .then((resp) => {
          if (resp.status == 204) {
            this.obtenerReservas();
          }
          if (resp.status == 204) {
            this.obtenerReservas();
          }
        });
      console.log(this.ObtenerEntidades);
    },
    RecuperarReservas: async function (id) {
      let Aidi = parseInt(id);
      let estado = { estado: true };

      await axios
        .put("http://localhost:3000/reserva/cambioEstado/" + Aidi, estado)
        .then((resp) => {
          if (resp.status == 204) {
            this.obtenerReservas();
          }
        });
      console.log(this.ObtenerEntidades);
    },

    enviarReservas: async function () {
      if (this.reservar.fechaEntrada == "") {
        alert("Complete Las fechas de la reserva.");
        return 0;
      }
      // if (this.fechaSalida == "") {
      //   alert("Complete Las fechas de la reserva.");
      //   return 0;
      // }
      if (this.reservar.idusuario == "") {
        alert("Dato de reserva añadidio correctamente.");
        return 0;
      }
      this.reservar.cantPersonas = parseInt(this.reservar.cantPersonas);
      console.log("Hola");
      await axios
        .post("http://localhost:3000/reserva/create", this.reservar)
        .then((resp) => {
          if (resp.status == 201) {
            alert("Reserva hecha correctamente.");
            location.reload();
          }
        });
      console.log(this.reservar);
    },
    obtenerReservas: async function () {
      console.log("Hola");
      await axios.get("http://localhost:3000/reserva").then((resp) => {
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
  },
  setup() {},
};
</script>

<style scoped>
#a{
  padding-top: 14%;
}

#text-title {
  margin-top: 20px;
  margin-left: 40%;
}
</style>

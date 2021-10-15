<template>
  <div>
    <div class="row">
       <div class="col-3">
          <b-alert
          :show="dismissCountDown"
          dismissible
          :variant="mensaje.color"
          @dismissed="dismissCountDown=0"
          @dismiss-count-down="countDownChanged">
           {{mensaje.texto}}
          </b-alert>

 
         <form @submit.prevent="agregarususario(notaEditar)" v-if="!editar">
           <div class="espacios">
                <div>
                    <p>

                    </p>
                </div>
                <div>
                    <input type="text" id="nom" placeholder=" USUARIO" required class="usu" v-model="obrero.usuario">
                </div>
              
                <div>
                    <p></p>
                </div>
                <div>
                    <input type="text" id="ape" placeholder="CONTRASEÑA" required class="usu" v-model="obrero.contraseña">
                </div>
                <div>
                <b-button v-b-modal.modal-1 variant="outline-primary" type="submit" class="btn my-2">Añadir</b-button>
            </div>
                
            </div>
         </form>
          <form @submit.prevent="editarususario()" v-if="editar">
           <div class="espacios">
                <div>
                    <p>

                    </p>
                </div>
                <div>
                    <input type="text" id="nom" placeholder=" USUARIO" required class="usu" v-model="notaEditar.usuario">
                </div>
              
                <div>
                    <p></p>
                </div>
                <div>
                    <input type="text" id="ape" placeholder="CONTRASEÑA" required class="usu" v-model="notaEditar.contraseña">
                </div>
                <div>
                <b-button   type="submit" class="btn my-2 mx-2">Editar</b-button>
                <b-button   type="submit" class="btn my-2" @click="editar=false">Cancelar</b-button>
            </div>
                
            </div>
         </form>
            
            <div>
                <p></p>
            </div>
            <div class="recuadro">
                <ul class="list-group">
                    <li class="list-group-item">
                      <input class="form-check-input me-1" type="checkbox" value="" aria-label="...">
                      Precios
                    </li>
                    <li class="list-group-item">
                      <input class="form-check-input me-1" type="checkbox" value="" aria-label="...">
                      Productos
                    </li>
                    <li class="list-group-item">
                      <input class="form-check-input me-1" type="checkbox" value="" aria-label="...">
                      Sup. Usuario
                    </li>
                    <li class="list-group-item">
                      <input class="form-check-input me-1" type="checkbox" value="" aria-label="...">
                      Contactos
                    </li>
                    <li class="list-group-item">
                      <input class="form-check-input me-1" type="checkbox" value="" aria-label="...">
                      Recomendar
                    </li>
                    <li class="list-group-item">
                        <input class="form-check-input me-1" type="checkbox" value="" aria-label="...">
                        Adopción
                      </li>
                  </ul>
            </div>
            <div>
                <p></p>
            </div>
            
       </div>
       <div class="col-9">
           <div>
                    <p>

                    </p>
                </div>
                <div>
                    <p>

                    </p>
                </div>
         <table class="table">
  <thead>
    <tr>
      <th scope="col">id</th>
      <th scope="col">usuario</th>
      <th scope="col">contraseña</th>
      <th scope="col">acción</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(item, index) in items" :key="index">
      <th scope="row">{{item._id}}</th>
      <td>{{item.Usuario}}</td>
      <td>{{item.Contraseña}}</td>
      <td>
        <b-button class="btn-danger mx-2" @click="eliminarnota(item._id)">Eliminar</b-button>
        <b-button class="btn-warning mx-2" @click="activarEdicion(item._id)">Editar</b-button>
      </td>
    </tr>
    
  </tbody>
</table>
       </div>
       
       
    </div>
  </div>
</template>
<script>
//import axios from 'axios'
  export default {
    data() {
      
      return {
        
        items: [],
        mensaje: {color: 'success', texto: ''},
        dismissSecs: 5,
        dismissCountDown: 0,
        obrero: {usuario:"", contraseña:""},
        editar:false,
        notaEditar:{}
        
      }
    },
    created(){
      this.listarItems();
    },
    methods: {
      listarItems(){

        this.axios.get('/nota')
        .then(res=>{

          this.items=res.data;

        })
        .catch(e=>{
          console.log(e.response)
        })

      },
      agregarususario(){

        this.axios.post('/nueva-nota', this.obrero)
        .them(res=>{

          this.items.push(res.data)
          this.obrero.usuario="";
          this.obrero.contraseña="";
          this.mensaje.color="success"
          this.mensaje.texto="ususario agregado";
          this.showAlert();
        })
        .catch(e=>{
          console.log(e.response)

        })

      },
      eliminarnota(){
        this.axios.delete('/nota/${id}')
        .then(res=>{
          const index = this.items.findIndex(item_id=> item_id===res.data._id);
          this.items.splice(index, 1);
          this.mensaje.color="success"
          this.mensaje.texto="nota eliminada";
          this.showAlert();

        })
        .catch(e=>{
          console.log(e.response);
        })
      },
      activarEdicion(){
        this.editar=true;
        this.axios.get('/nota/${id}')
        .then(res=>{
          this.notaEditar=res.data;
        })
        .catch(e=>{
          console.log(e.response);
        })
      },
      editarNota(item){
        this.axios.put('/nota/${item_id}', item)
        .them(res=>{
          const index =this.items.findIndex(n=> n._id===res.data._id);
          this.items[index].usuario=res.data.usuario;
          this.items[index].contraseña=res.data.contraseña;
          this.mensaje.color="success"
          this.mensaje.texto="nota editada";
          this.showAlert();
          this.editar=false;
        })
        .catch(e=>{
          console.log(e.response);
        })
      },
      countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown
      },
      showAlert() {
      this.dismissCountDown = this.dismissSecs
      },




      onSubmit(event) {
        event.preventDefault()
        /* alert(JSON.stringify(this.form)) */
        this.$swal('Hello Vue world!!!')
        this.$swal({
          position: 'top-end',
          icon: 'success',
          title: 'Your work has been saved',
          showConfirmButton: false,
          timer: 2000 
        })
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.food = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }
  }
</script>
<style scoped>
        .row{
            text-align: center;
        }
        .recuadro{
            text-align:left;
            justify-items:baseline;
        }
        .espacios{
            flex-direction: column;
        }
        .list-group-item{
            /* border-color: white; */
            width: 90%;
            padding: 5px;
            height: auto;
        }
        .list-group{
            margin-right:0px;
            margin-left: 70px;
        }
        .table{
            width: 70%;
            padding: 5px;
            margin-right:0px;
            margin-left: 150px;
            border: 5px;
            border-style: solid black;
            
        }
        .editar{
            width: 15%;
            padding: 5px;
        }
        .nomusu{
            text-align: left;
            /* width: 30%; */
        }
</style>
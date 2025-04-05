<template>
  <div>
    <h1 class="text-center">Agenda de contactos</h1>
    <hr/>
    <!-- Button trigger modal -->
    <button @click="update=false; openModal()" type="button" class="btn btn-primary">
      Nuevo contacto
    </button>

    <!-- Modal -->
    <div class="modal" :class="{show:modal}">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{titleModal}}</h5>
            <button  @click="closeModal()" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <div>
              <label for="first_name">Nombre</label>
              <input v-model="contact.first_name" type="text" class="form-control" id="first_name" placeholder="Nombre del contacto" name="">

              <label for="last_name">Apellido</label>
              <input v-model="contact.last_name" type="text" class="form-control" id="last_name" placeholder="Apellido" name="">
              
              <label for="email">Correo</label>
              <input v-model="contact.email" type="text" class="form-control" id="email" placeholder="Correo Electronic" name="">
              
              <label for="phone">Telefono</label>
              <input v-model="contact.phone" type="text" class="form-control" id="phone" placeholder="Telefono" name="">
              
              <label for="address">Domicilio</label>
              <input v-model="contact.address" type="text" class="form-control" id="address" placeholder="Dirección" name="">
            </div>
          </div>
          <div class="modal-footer">
            <button  @click="closeModal()" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
            <button  @click="save()" type="button" class="btn btn-success">Guardar cambios</button>
          </div>
        </div>
      </div>
    </div> 

    <table class="table table-hover table-bordered">
      <thead class="table-dark">
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Nombre</th>
          <th scope="col">Apellido</th>
          <th scope="col">Correo</th>
          <th scope="col">Telefono</th>
          <th scope="col">Dirección</th>
          <th scope="col" colspan="2" class="text-center">Acción</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <th scope="row">{{contact.id}}</th>
          <td>{{contact.first_name}}</td>
          <td>{{contact.last_name}}</td>
          <td>{{contact.email}}</td>
          <td>{{contact.phone}}</td>
          <td>{{contact.address}}</td>
          <td>
            <button @click="update=true; openModal(contact);" class="btn btn-warning">Editar</button>
          </td>
          <td>
            <button @click="eliminar(contact.id)" class="btn btn-danger">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                contact:{
                  first_name:'',
                  last_name:'',
                  email:'', 
                  phone:'',
                  address:'',
                },
                id:0,
                update: true,
                modal:0,
                titleModal: '',
                contacts: [], 
            }
        },
        methods: {
            async list(){
                const res = await axios.get('contacts');
                this.contacts = res.data
            },
            async eliminar(id){
              const res = await axios.delete('/contacts/' +id);
              this.list(); 
            },
            async save(){
              if(this.update){
                  const res = await axios.put('/contacts/' + this.id, this.contact);
              }else{
                const res = await axios.post('/contacts', this.contact);
              }
              this.closeModal();
              this.list();
            },
            openModal(data={}){
              this.modal=1;
              if(this.update){
                this.id = data.id,
                this.titleModal="Modificar Contacto";
                this.contact.first_name=data.first_name;
                this.contact.last_name=data.last_name;
                this.contact.email=data.email;
                this.contact.phone=data.phone;
                this.contact.address=data.address;
              }else{
                this.id = 0,
                this.titleModal="Crear Contacto";
                this.contact.first_name='';
                this.contact.last_name='';
                this.contact.email='';
                this.contact.phone='';
                this.contact.address = '';
              }
            },
            closeModal(id){
              this.modal=0; 
            },

        }, 
        created(){
            this.list();
        }
    };
</script>
<style>
.show{
  display: list-item;
  opacity: 1;
  background: rgba(51, 196, 36, 0.849);
}
</style>
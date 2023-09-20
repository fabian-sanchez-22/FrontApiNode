
<template lang="" >
      <div class="superhero-form">
      <h2>Crear Superhéroe</h2>
      <form>
        <div class="form-group">
          <label for="nombre">Nombre:</label>
          <input type="text" id="txtNombre" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="apellido">Apellido:</label>
          <input type="text" id="txtApellido" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="nombre_heroe">Nombre de Héroe:</label>
          <input type="text" id="txtNombre_heroe" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="ciudad">Ciudad:</label>
          <input type="text" id="txtCiudad" class="input-field" required>
        </div>
        <button type="button" class="btn-create" @click="create()" >Crear Superhéroe</button>
      </form>
    </div>


    <table class="table">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">Id</th>
      <th scope="col">Nombre</th>
      <th scope="col">Apellido</th>
      <th scope="col">Nombre_Heroe</th>
      <th scope="col">Ciudad</th>
    </tr>
  </thead>
  <tbody id="tableBody">
    <tr v-for="(element,index) in arraySuperheroe">
      <td v-text="index+1"></td>
      <td v-text="element.id"></td>
      <td v-text="element.nombre"></td>
      <td v-text="element.apellido"></td>
      <td v-text="element.nombre_heroe"></td>
      <td v-text="element.ciudad"></td>
      <td><button @click="deletes(element.id)">Eliminar</button></td>
      <button @click="readUpdate(element.id)" type="button" class="btn" data-bs-toggle="modal" data-bs-target="#updateModal">
  modificar
</button>
    </tr>
  </tbody>
</table>


<div class="modal fade" id="updateModal" tabindex="-1" aria-labelledby="updateModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="updateModalLabel">Modificar superheroe</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div class="form-group">
          <label for="nombre">Nombre:</label>
          <input type="text" id="txtNombreModal" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="apellido">Apellido:</label>
          <input type="text" id="txtApellidoModal" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="nombre_heroe">Nombre de Héroe:</label>
          <input type="text" id="txtNombre_heroeModal" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="ciudad">Ciudad:</label>
          <input type="text" id="txtCiudadModal" class="input-field" required>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
        <button type="button" class="btn btn-primary" @click="update()" data-bs-dismiss="modal">Modificar</button>
      </div>
    </div>
  </div>
</div>

</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      arraySuperheroe: [],
      id: ""
    }
  },

  methods: {
    create() {
      axios.post("http://localhost:3000/api/v1/superheroe", {
        nombre: txtNombre.value,
        apellido: txtApellido.value,
        nombre_heroe: txtNombre_heroe.value,
        ciudad: txtCiudad.value
      })
        .then(res => {
          console.log(res);
          this.read()
          this.clear()
        })
        .catch(err => {
          console.log(err);
        })
    },

    clear(){
      txtNombre.value = ""
      txtApellido.value = ""
      txtNombre_heroe.value = ""
      txtCiudad.value = ""


      txtNombreModal.value = ""
          txtApellidoModal.value =  ""
          txtNombre_heroeModal.value = ""
          txtCiudadModal.value = ""
    },


    read() {
      axios.get("http://localhost:3000/api/v1/superheroe")
        .then(res => {
          this.arraySuperheroe = res.data
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        })
    },

    readUpdate(id) {
      axios.get("http://localhost:3000/api/v1/superheroe/" + id)
        .then(res => {
          console.log(res.data);
          txtNombreModal.value = res.data[0].nombre
          txtApellidoModal.value = res.data[0].apellido
          txtNombre_heroeModal.value = res.data[0].nombre_heroe
          txtCiudadModal.value = res.data[0].ciudad

          this.id = res.data[0].id
        })
        .catch(err => {
          console.log(err);
        })

    },

    update() {
      console.log(this.id);
      axios.put("http://localhost:3000/api/v1/superheroe/" + this.id, {
        nombre: txtNombreModal.value,
        apellido: txtApellidoModal.value,
        nombre_heroe: txtNombre_heroeModal.value,
        ciudad: txtCiudadModal.value
      })
        .then(res => {
          console.log(res);
          this.read()
          this.clear()
        })
        .catch(err => {
          console.log(err);
        })
    },

    deletes(id) {
      if(confirm("Desea eliminar el registro ?") == true) {
      axios.delete("http://localhost:3000/api/v1/superheroe/" + id)
        .then(res => {
          console.log(res);
          console.log(id);
          this.read()
        })
        .catch(err => {
          console.log(err);
        })
    }
  }

  },

  mounted() {
    this.read()
  },
}
</script>






<!-- <template>
  <div>
    <div class="superhero-form">
      <h2>Crear Superhéroe</h2>
      <form @submit.prevent="createSuperhero">
        <div class="form-group">
          <label for="nombre">Nombre:</label>
          <input type="text" v-model="newSuperhero.nombre" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="apellido">Apellido:</label>
          <input type="text" v-model="newSuperhero.apellido" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="nombre_heroe">Nombre de Héroe:</label>
          <input type="text" v-model="newSuperhero.nombre_heroe" class="input-field" required>
        </div>
        <div class="form-group">
          <label for="ciudad">Ciudad:</label>
          <input type="text" v-model="newSuperhero.ciudad" class="input-field" required>
        </div>
        <button type="submit" class="btn-create">Crear Superhéroe</button>
      </form>
    </div>

    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Id</th>
          <th scope="col">Nombre</th>
          <th scope="col">Apellido</th>
          <th scope="col">Nombre_Heroe</th>
          <th scope="col">Ciudad</th>
        </tr>
      </thead>
      <tbody id="tableBody">
        <tr v-for="(element, index) in arraySuperheroe" :key="element.id">
          <td>{{ index + 1 }}</td>
          <td>{{ element.id }}</td>
          <td>{{ element.nombre }}</td>
          <td>{{ element.apellido }}</td>
          <td>{{ element.nombre_heroe }}</td>
          <td>{{ element.ciudad }}</td>
          <td><button @click="deleteSuperhero(element.id)">Eliminar</button></td>
          <td>
            <button @click="openUpdateModal(element.id)" type="button" class="btn" data-bs-toggle="modal" data-bs-target="#updateModal">
              Modificar
            </button>
          </td>
        </tr>
      </tbody>
    </table>

 
    <div class="modal fade" id="updateModal" tabindex="-1" aria-labelledby="updateModalLabel" aria-hidden="true">
      
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      arraySuperheroe: [],
      newSuperhero: {
        nombre: '',
        apellido: '',
        nombre_heroe: '',
        ciudad: '',
      },
      selectedSuperheroId: '',
      updateSuperhero: {
        nombre: '',
        apellido: '',
        nombre_heroe: '',
        ciudad: '',
      },
    };
  },

  methods: {
    createSuperhero() {
      axios
        .post('http://localhost:3000/api/v1/superheroe', this.newSuperhero)
        .then((res) => {
          console.log(res);
          this.readSuperheroes();
        })
        .catch((err) => {
          console.log(err);
        });
    },

    readSuperheroes() {
      axios
        .get('http://localhost:3000/api/v1/superheroe')
        .then((res) => {
          this.arraySuperheroe = res.data;
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
        });
    },

    openUpdateModal(id) {
      axios
        .get('http://localhost:3000/api/v1/superheroe/' + id)
        .then((res) => {
          this.updateSuperhero = res.data[0];
          this.selectedSuperheroId = id;
        })
        .catch((err) => {
          console.log(err);
        });
    },

    updateSuperhero() {
      axios
        .put(
          'http://localhost:3000/api/v1/superheroe/' + this.selectedSuperheroId,
          this.updateSuperhero
        )
        .then((res) => {
          console.log(res);
          this.readSuperheroes();
        })
        .catch((err) => {
          console.log(err);
        });
    },

    deleteSuperhero(id) {
      axios
        .delete('http://localhost:3000/api/v1/superheroe/' + id)
        .then((res) => {
          console.log(res);
          this.readSuperheroes();
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },

  mounted() {
    this.readSuperheroes();
  },
};
</script>  -->








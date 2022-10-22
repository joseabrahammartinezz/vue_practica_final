<template>


    <div class="container">
        <label class="form-label">Agregar Area</label>
      
        <section class="form">
            <form action="" class="text-center">
                <input v-model="area.nombre" @keyup.enter="agregarArea" type="text" name="nombre" class="form-control" placeholder="Nombre">
                <input v-model="area.encargado" @keyup.enter="agregarArea" type="text" name="encargado" placeholder="Encargado" class="form-control">
                <input v-model="area.num_funcionarios" @keyup.enter="agregarArea" type="number" name="numfuncionarios" placeholder="Número de Funcionarios" class="form-control">
                <!-- Botón para añadir -->
                <input @click="agregarArea" type="button" value="Añadir" class="btn btn-success">
            </form>
        </section>
        <!-- Tabla donde se muestran los datos -->
        
        <section class="data">
        <label class="form-label">Listado de Areas</label>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">id</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Encargado</th>
                    <th scope="col"># Funcionarios</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(area, index) in areas">
                    <td>{{ area.id }}</td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="nombreActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            <!-- Dato nombre -->
                            {{ area.nombre }}
                        </span>
                    </td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="encargadoActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            <!-- Dato edad -->
                            {{ area.encargado }}
                        </span>
                    </td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="numfuncionariosActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            <!-- Dato edad -->
                            {{ area.num_funcionarios }}
                        </span>
                    </td>
                    <td>
                        <!-- Botón para guardar la información actualizada -->
                        <span v-if="formActualizar && idActualizar == index">
                            <button @click="guardarActualizacion(index+1)" class="btn btn-success">Guardar</button>
                        </span>
                        <span v-else>
                            <!-- Botón para mostrar el formulario de actualizar -->
                            <button @click="verFormActualizar(index)" class="btn btn-warning">Actualizar</button>
                            <!-- Botón para borrar -->
                            <button @click="borrarArea(index+1, area.nombre)" class="btn btn-danger">Borrar</button>
                        </span>
                    </td>
                </tr>
            </tbody>
        </table>
    </section>
    </div>
  
</template>

<script>
export default {
    name: 'AreasView',
    data() {
        return {
            // Ver o no ver el formulario de actualizar
            formActualizar: false,
            // La posición de tu lista donde te gustaría actualizar 
            idActualizar: -1,
            // Input nombre dentro del formulario de actualizar
            nombreActualizar: '',
            // Input encargado dentro del formulario de actualizar
            encargadoActualizar: '',
            // Input # funcionarios dentro del formulario de actualizar
            numfuncionariosActualizar: '',
            // datos del area
            area:{
                id: 0,
                nombre:null,
                encargado: null,
                num_funcionarios: 0
            },
            // lista de areas
            areas: []
        }
    },
    methods: {
        getAreas(){
            axios({
                method: "get",
                url: "http://localhost:3333/Areas/"
            })
            .then(response => {
                this.areas = response.data;
            console.log(response);
            })
            .catch(e => console.log(e));
        },
        agregarArea() {
            axios({
                method: "post",
                url: "http://localhost:3333/Areas/",
                data: this.area
            })
                .then(response => {
                    console.log(response);
                    this.getAreas();
                    location.reload();
                })
                .catch(e => console.log(e));
        },
        verFormActualizar(area_id) {
                // Antes de mostrar el formulario de actualizar, rellenamos sus campos
                console.log("numero del indice "+ area_id);
                console.log("nombre "+ this.areas[area_id].nombre);
                this.idActualizar = area_id;
                this.nombreActualizar = this.areas[area_id].nombre;
                this.encargadoActualizar = this.areas[area_id].encargado;
                this.numfuncionariosActualizar = this.areas[area_id].num_funcionarios;
                // Mostramos el formulario
                this.formActualizar = true;
        },
        borrarArea (area_id, nombre_area) {
            if (confirm("Esta seguro de eliminar el Area: " + nombre_area + "?")) {
                    axios({
                        method: "delete",
                        url: "http://localhost:3333/Areas/" + area_id
                    })
                        .then(response => {
                            this.getTareas();
                            console.log(response);
                        })
                        .catch(e => console.log(e));
                    location.reload();
                }
        },
        guardarActualizacion(area_id) {
            this.area.id=area_id;
            this.area.nombre=this.nombreActualizar;
            this.area.encargado=this.encargadoActualizar;
            this.area.num_funcionarios= this.numfuncionariosActualizar;
            console.log(this.area);
            axios({
                method: "patch",
                url: "http://localhost:3333/Areas/"+area_id,
                data: this.area
            })
            .then(response => {
                console.log("Se actualizó el Area exitosamente"+ area_id);
            })
            .catch(e => console.log(e));
            
            this.formActualizar = false;
            this.getAreas();
            location.reload();
        }
    },
    computed: {
    },
    mounted() {
        this.getAreas()
    },
    components: {
    }
}
</script>

<style scoped>
form {
    max-width: 400px;
}
</style>
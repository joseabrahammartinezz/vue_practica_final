<template>


    <div class="container">
        <label class="form-label">Agregar Activo</label>
        <section class="form">
            <form action="" class="text-center">
                <input v-model="activo.tipo_activo" @keyup.enter="agregarActivo" type="text" name="tipo_activo" class="form-control" placeholder="Tipo Activo">
                <input v-model="activo.marca" @keyup.enter="agregarActivo" type="text" name="marca" placeholder="Marca" class="form-control">
                <input v-model="activo.modelo" @keyup.enter="agregarActivo" type="text" name="modelo" placeholder="Modelo" class="form-control">
                <input v-model="activo.estado" @keyup.enter="agregarActivo" type="text" name="estado" placeholder="Estado" class="form-control">
                <input v-model="activo.areaId" @keyup.enter="agregarActivo" type="number" name="areaid" placeholder="ID del Area" class="form-control">
                <!-- Botón para añadir -->
                <input @click="agregarActivo" type="button" value="Añadir" class="btn btn-success">
            </form>
        </section>
        <!-- Tabla donde se muestran los datos -->
        
        <section class="data">
        <label class="form-label">Listado de Activos</label>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">id</th>
                    <th scope="col">Tipo Activo</th>
                    <th scope="col">Marca</th>
                    <th scope="col">Modelo</th>
                    <th scope="col">Estado</th>
                    <th scope="col">Id del Area</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(activo, index) in activos">
                    <td>{{ activo.id }}</td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="tipoactivoActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            {{ activo.tipo_activo }}
                        </span>
                    </td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="marcaActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            {{ activo.marca }}
                        </span>
                    </td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="modeloActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            {{ activo.modelo }}
                        </span>
                    </td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="estadoActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            {{ activo.estado }}
                        </span>
                    </td>
                    <td>
                        <span v-if="formActualizar && idActualizar == index">
                            <!-- Formulario para actualizar -->
                            <input v-model="areaidActualizar" type="text" class="form-control">
                        </span>
                        <span v-else>
                            {{ activo.areaId }}
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
                            <button @click="borrarActivo(index+1, activo.tipo_activo +  ' ' + activo.marca + ' ' + activo.modelo)" class="btn btn-danger">Borrar</button>
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
    name: 'ActivosView',
    data() {
        return {
            // Ver o no ver el formulario de actualizar
            formActualizar: false,
            // La posición de tu lista donde te gustaría actualizar 
            idActualizar: -1,
            // Input nombre dentro del formulario de actualizar
            tipoactivoActualizar: '',
            // Input encargado dentro del formulario de actualizar
            marcaActualizar: '',
            // Input # funcionarios dentro del formulario de actualizar
            modeloActualizar: '',
            estadoActualizar: '',
            areaidActualizar: '',
            // datos del activo
            activo:{
                id: 0,
                tipo_activo:null,
                marca: null,
                modelo: null,
                estado: null,
                areaId:0
            },
            // lista de areas
            activos: []
        }
    },
    methods: {
        getActivos(){
            axios({
                method: "get",
                url: "http://localhost:3333/Activos/"
            })
            .then(response => {
                this.activos = response.data;
            console.log(response);
            })
            .catch(e => console.log(e));
        },
        agregarActivo() {
            axios({
                method: "post",
                url: "http://localhost:3333/Activos/",
                data: this.activo
            })
                .then(response => {
                    console.log(response);
                    this.getActivos();
                    location.reload();
                })
                .catch(e => console.log(e));
        },
        verFormActualizar(activo_id) {
                // Antes de mostrar el formulario de actualizar, rellenamos sus campos
                console.log("numero del indice "+ activo_id);
                console.log("nombre "+ this.activos[activo_id].nombre);
                this.idActualizar = activo_id;
                this.tipoactivoActualizar = this.activos[activo_id].tipo_activo;
                this.marcaActualizar = this.activos[activo_id].marca;
                this.modeloActualizar = this.activos[activo_id].modelo;
                this.estadoActualizar = this.activos[activo_id].estado;
                this.areaidActualizar= this.activos[activo_id].areaId;
                // Mostramos el formulario
                this.formActualizar = true;
        },
        borrarActivo (activo_id, nombre_activo) {
            if (confirm("Esta seguro de eliminar el Activo: " + nombre_activo + "?")) {
                    axios({
                        method: "delete",
                        url: "http://localhost:3333/Activos/" + activo_id
                    })
                        .then(response => {
                            this.getActivos();
                            console.log(response);
                            location.reload();
                        })
                        .catch(e => console.log(e));
                    
                }
        },
        guardarActualizacion(activo_id) {
            this.activo.id=activo_id;
            this.activo.tipo_activo=this.tipoactivoActualizar;
            this.activo.marca=this.marcaActualizar;
            this.activo.modelo= this.modeloActualizar;
            this.activo.estado= this.estadoActualizar;
            this.activo.areaId= this.areaidActualizar;
            console.log(this.activo);
            axios({
                method: "patch",
                url: "http://localhost:3333/Activos/"+activo_id,
                data: this.activo
            })
            .then(response => {
                console.log("Se actualizó el Activo exitosamente");
            })
            .catch(e => console.log(e));
            
            this.formActualizar = false;
            this.getActivos();
            location.reload();
        }
    },
    computed: {
    },
    mounted() {
        this.getActivos()
    },
    components: {
    }
}
</script>

<style scoped>
form {
    max-width: 600px;
}
</style>
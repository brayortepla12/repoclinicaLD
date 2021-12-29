<template>
    <div class="container">
        <h1 class="text-center"> GESTION DE USUARIOS </h1>
        <hr>
        <!-- Button trigger modal -->
        <button @click="modificar=flase; abrirmodal();" type="button" class="btn btn-primary">
        Agregar
        </button>

        <!-- Modal -->
        <div class="modal" :class="{mostrarmodal:estadomodal}">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="staticBackdropLabel">{{titulomodal}}</h5>
                <button @click="cerrarmodal();" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <form>
                    <div class="mb-3">
                        <label for="nombre" class="form-label">Nombre</label>
                        <input v-model="elusario.nombre" type="text" class="form-control" id="nombre" placeholder="Nombre">
                    </div>
                    <div class="mb-3">
                        <label for="usuario" class="form-label">Usuario</label>
                        <input  v-model="elusario.usuario" type="text" class="form-control" id="usuario" placeholder="Usuario">
                    </div>
                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">Correo</label>
                        <input  v-model="elusario.correo" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                    </div>
                    <div class="mb-3">
                        <label for="exampleInputPassword1" class="form-label">Contraseña</label>
                        <input  v-model="elusario.clave" type="password" class="form-control" id="exampleInputPassword1">
                    </div>
                    <div class="mb-3">
                        <label for="rol" class="form-label">Seleccione el rol</label>
                        <select  v-model="elusario.rol_id" id="rol" class="form-select">
                            <option>Seleccionar</option>
                            <option>1</option>
                            <option>2</option>
                            <option>3</option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <label for="estado" class="form-label">Seleccione el estado</label>
                        <select  v-model="elusario.estado" id="estado" class="form-select">
                            <option>Seleccionar</option>
                            <option>0</option>
                            <option>1</option>
                        </select>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button @click="cerrarmodal();" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                <button @click="guardar();" type="button" class="btn btn-success">Guardar</button>
            </div>
            </div>
        </div>
        </div>
        <br>
        <br>
        <table class="table table-striped">
            <thead class="thead-dark">
                <tr>
                <th scope="col">Nombre</th>
                <th scope="col">Usuario</th>
                <th scope="col">Rol</th>
                <th scope="col">Estado</th>
                <th scope="col">Email</th>
                <th scope="col">Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="usuario in usuarios" :key="usuario.id">
                    <td>{{usuario.nombre}}</td>
                    <td>{{usuario.usuario}}</td>
                    <td>{{usuario.rol_id}}</td>
                    <td>{{usuario.estado}}</td>
                    <td>{{usuario.correo}}</td>
                    <td>
                        <button @click="modificar=true; abrirmodal(usuario);" type="button" class="btn btn-default">
                            <i class="fas fa-pencil-alt"></i>
                        </button>
                        <button @click="eliminar(usuario.id)" type="button" class="btn btn-default">
                            <i class="fas fa-trash"></i>
                        </button>
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
                elusario:{
                    nombre: "",
                    usuario: "",
                    correo: "",
                    clave: "",
                    estado: "",
                    rol: "",

                },
                id:0,
                estadomodal:0,
                titulomodal : '',
                usuarios:[],
                modificar:true,
            }
        },
        methods:{
            async listar(){
                const res = await axios.get('/usuarios')
                this.usuarios = res.data;
            },
            async eliminar(id){
                const res = await axios.delete('/usuarios/'+id)
                this.listar();
            },
            abrirmodal(data={}){
                this.estadomodal = 1;
                if(this.modificar){
                    this.id = data.id,
                    this.titulomodal ="Modificar Usuario"; 
                    this.elusario.nombre = data.nombre;
                    this.elusario.usuario = data.usuario;
                    this.elusario.correo = data.correo;
                    this.elusario.clave = data.clave;
                    this.elusario.estado = data.estado;
                    this.elusario.rol = data.rol;
                }else{
                    this.id = 0;
                    this.titulomodal ="Crear Usuario";
                    this.elusario.nombre = "";
                    this.elusario.usuario = "";
                    this.elusario.correo = "";
                    this.elusario.clave = "";
                    this.elusario.estado = "";
                    this.elusario.rol = "";
                }

            },
            cerrarmodal(){
                this.estadomodal = 0;
            },
            async guardar(){
                if(this.modificar){
                    const res = await axios.put('/usuarios/' + this.id,this.elusario);
                }else{
                    const res = await axios.post('/usuarios/',this.elusario);
                }
                this.cerrarmodal();
                this.listar();
            },

        },
        created(){
            this.listar();
        },
    }
</script>

<style>
.mostrarmodal{
    display: list-item;
    opacity: 1;
    background: rgb(44, 38, 75, 0.849);
}
</style>

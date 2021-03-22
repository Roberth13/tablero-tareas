<template>
    <div>
        <v-row>
            <v-col cols="12" md="3" sm="3" class="text-left">
                <v-select v-model="proyecto" :items="misProyectos" item-text="projectName" item-value="id" dense outlined hint="Mis proyectos" persistent-hint></v-select>
            </v-col>
            <v-col cols="12" md="9" sm="9" class="text-right">
                <v-btn
                    dark
                    color="primary"
                    class="mr-2"
                    @click="dashboard"
                    >
                    <v-icon dark>
                        mdi-history
                    </v-icon>
                    <span>Dashboard</span>
                </v-btn>
                <v-btn
                    dark
                    color="warning"
                    class="mr-2"
                    @click="historial"
                    >
                    <v-icon dark>
                        mdi-history
                    </v-icon>
                    <span>Historial</span>
                </v-btn>
                <v-btn
                    dark
                    color="indigo"
                    class="mr-2"
                    @click="tareas"
                    >
                    <v-icon dark>
                        mdi-format-list-checks
                    </v-icon>
                    <span>Mis Tareas</span>
                </v-btn>
                <v-btn
                    dark
                    color="success"
                    class="mr-2"
                    @click="crearTarea"
                    >
                    <v-icon dark>
                        mdi-plus
                    </v-icon>
                    <span>Crear Tarea</span>
                </v-btn>
                <v-btn
                    dark
                    color="error"
                    @click="salir"
                    >
                    <v-icon dark>
                        mdi-exit-to-app
                    </v-icon>
                    <span>Salir</span>
                </v-btn>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12">
                <v-dialog v-model="dialogTarea" persistent max-width="500">
                    <v-card>
                        <v-card-title style="background:#ccc" class="mb-4">
                            Crear Tarea
                        </v-card-title>
                        <v-card-text>
                           <v-form ref="form" v-model="valid" >
                                <v-row>
                                    <v-col cols="12">
                                        <v-text-field hint="Ingrese el titulo" dense outlined persistent-hint label="Titulo"
                                        v-model="titulo" :rules="[v => !!v || 'El titulo es requerido']" required></v-text-field>
                                    </v-col>
                                    <v-col cols="12">
                                        <v-text-field hint="Ingrese un comentario" dense outlined persistent-hint label="Comentario" 
                                        v-model="comentario" ></v-text-field>
                                    </v-col>
                                </v-row>
                            </v-form>
                        </v-card-text>
                        <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="green darken-1" text  @click="dialogTarea = false"  >
                            Cancelar
                        </v-btn>
                        <v-btn
                            color="green darken-1" text
                            @click="crear" :disabled="!valid">
                            Crear
                        </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-col>
        </v-row>
        <v-snackbar v-model="snack" :color="color_snack">{{ mensaje }}</v-snackbar>
    </div>
</template>
<script>
import axios from "axios";
export default {
    name:'Header',
    data(){
        return{
            dialogTarea: false,
            titulo: '',
            comentario: '',
            valid: false,
            misProyectos: [],
            access_token: '',
            proyecto: '',
            noProyecto: true,
            color_snack: 'success',
            mensaje: '',
            snack: false,
        }
    },
    methods:{
        historial(){
            if(!this.noProyecto){
                this.$router.push({
                    name: 'historial',
                    params: { id: this.proyecto }
                })
            }else{
                this.mostrarSnack('La compañia aún no ha registrado proyectos', 'warning');
            }
        },
        dashboard(){
            this.$router.push({
                name: 'dashboard'
            })
        },
        crearTarea(){
            if(!this.noProyecto){
                this.dialogTarea = true
            }else{
                this.mostrarSnack('La compañia aún no ha registrado proyectos', 'warning');
            }
            
        },
        tareas(){
            if(!this.noProyecto){
                this.$router.push({
                    name: 'tareas',
                    params: { id: this.proyecto }
                })
            }else{
                this.mostrarSnack('La compañia aún no ha registrado proyectos', 'warning');
            }
            
        },
        salir(){
            localStorage.removeItem('access_token');
            this.$router.push({
                path: '/'
            })
        },
        crear(){
            if(this.valid){
                let data = {
                    project_id: this.proyecto,
                    state_id: 1,
                    title: this.titulo,
                    comment: this.comentario
                };
                axios.post("http://localhost:8000/api/tasks", data, { headers: {"Authorization" : `Bearer ${this.access_token}`} }).then((result) => {
                    if(result.data.success == 1){
                        this.mostrarSnack('Tarea creada con éxito', 'success');
                        this.dialogTarea = false;
                        this.tareas();
                    }
                })
            }
        },
        getProjects(){
            axios.get("http://localhost:8000/api/projects", { headers: {"Authorization" : `Bearer ${this.access_token}`} }).then((result) => {
                if(result.data.success == 1){
                    this.misProyectos = result.data.data;
                    if(this.misProyectos.length > 0){
                        let _sel = this.misProyectos.slice(0, 1);
                        this.proyecto = _sel[0].id;
                        this.noProyecto = false;
                    }
                   
                }
            })
        },
        mostrarSnack(mensaje, color){
            this.mensaje = mensaje;
            this.snack = true;
            this.color_snack = color;
            setTimeout(() =>{
                this.snack = false;
            }, 3000);
        }
    },
    created() {
        this.access_token = localStorage.getItem('access_token');
        if(localStorage.getItem('access_token') != null){
            this.access_token = JSON.parse(localStorage.getItem('access_token'));
            this.getProjects();            
        }        
    }
}
</script>
<style scoped>

</style>
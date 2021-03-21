<template>
    <div>
        <v-row>
            <v-col cols="12" md="3" sm="3" class="text-left">
                <v-select :items="misProyectos" dense outlined hint="Mis proyectos" persistent-hint></v-select>
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
                    @click="dialogTarea = true"
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
    </div>
</template>
<script>
export default {
    name:'Header',
    data(){
        return{
            dialogTarea: false,
            titulo: '',
            comentario: '',
            valid: false,
            misProyectos: []
        }
    },
    methods:{
        historial(){
            this.$router.push({
                path: 'historial'
            })
        },
        dashboard(){
            this.$router.push({
                path: 'dashboard'
            })
        },
        tareas(){
            this.$router.push({
                path: 'tareas'
            })
        },
        salir(){
            this.$router.push({
                path: '/'
            })
        },
        crear(){

        }
    }
}
</script>
<style scoped>

</style>
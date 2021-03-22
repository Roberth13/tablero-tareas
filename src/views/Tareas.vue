<template>
    <div>
        <v-container fluid> 
            <Header />
            <v-row>
                <v-col cols="12" class="text-center">
                    <h1>Mis Tareas</h1>
                </v-col>
            </v-row>
             <v-row>
                <v-col cols="12">
                    <v-data-table
                        :headers="headers"
                        :items="items"
                        :items-per-page="10"
                        class="elevation-4">
                        <template v-slot:top>
                        <v-dialog v-model="dialog" max-width="500px">
                                <v-card>
                                    <v-card-title>
                                        <span class="headline">Editar Tarea</span>
                                    </v-card-title>
                                    <v-card-text>
                                        <v-form ref="form" v-model="valid" >
                                            <v-row>
                                                <v-col cols="12">
                                                    <v-text-field hint="Ingrese el titulo" dense outlined persistent-hint label="Titulo"
                                                    v-model="titulo" :rules="[v => !!v || 'El titulo es requerido']" required></v-text-field>
                                                </v-col>
                                                <v-col cols="12">
                                                    <v-select hint="Seleccione el estado" dense outlined persistent-hint label="Estado"
                                                    :items="estados" item-text="name" item-value="id"
                                                    v-model="estado" :rules="[v => !!v || 'El estado es requerido']" required></v-select>
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
                                        <v-btn
                                            color="blue darken-1"
                                            text
                                            @click="close"
                                        >
                                            Cancelar
                                        </v-btn>
                                        <v-btn
                                            color="blue darken-1"
                                            text
                                            @click="save"
                                        >
                                            Guardar
                                        </v-btn>
                                    </v-card-actions>
                                </v-card>
                            </v-dialog>
                            <v-dialog v-model="dialogDelete" max-width="500px">
                                <v-card>
                                    <v-card-title class="headline">Estas seguro de eliminar esta tarea?</v-card-title>
                                    <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
                                    <v-btn color="blue darken-1" text @click="deleteItemConfirm">Eliminar</v-btn>
                                    <v-spacer></v-spacer>
                                    </v-card-actions>
                                </v-card>
                            </v-dialog>
                        </template>
                        <template v-slot:[`item.actions`]="{ item }">
                            <v-icon small class="mr-2" @click="editItem(item)" >
                                mdi-pencil
                            </v-icon>
                            <v-icon small @click="deleteItem(item)">
                                mdi-delete
                            </v-icon>
                        </template>
                    </v-data-table>
                </v-col>
            </v-row>
        </v-container>
        <v-snackbar v-model="snack" :color="color_snack">{{ mensaje }}</v-snackbar>
    </div>
</template>
<script>
import axios from "axios";
import Header from  '../components/Header';
export default {
    name:'Tareas',
    components:{
        Header
    },
    data(){
        return{
            headers: [
                { text: 'Tarea', value: 'title' },
                { text: 'Estado', value: 'status' },
                { text: 'Fecha', value: 'updated_at' },
                { text: 'Acciones', value: 'actions', sortable: false },
            ],
            items:[],
            access_token: '',
            dialog: false,
            dialogDelete: false,
            editedIndex: -1,
            editedItem:{
                comentario: '',
                estado: 0
            },
            defaultItem:{
                comentario: '',
                estado: 0
            },
            comentario: '',
            titulo: '',
            estado: 0,
            valid: false,
            color_snack: 'success',
            mensaje: '',
            snack: false,
            estados: [],
            idEdit:0
        }
    },
    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },
    created() {
        this.access_token = localStorage.getItem('access_token');
        if(localStorage.getItem('access_token') != null){
            this.access_token = JSON.parse(localStorage.getItem('access_token'));
            this.getTareas();   
            this.getEstados();         
        }        
    },
    methods:{
        getTareas(){
            axios.get("http://localhost:8000/api/tasks/my/"+this.$route.params.id, { headers: {"Authorization" : `Bearer ${this.access_token}`} }).then((result) => {
                if(result.data.success == 1){
                    this.items = result.data.data;                   
                }
            })
        },
        getEstados(){
            axios.get("http://localhost:8000/api/state/", { headers: {"Authorization" : `Bearer ${this.access_token}`} }).then((result) => {
                if(result.data.success == 1){
                    this.estados = result.data.data;                   
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
        },
        editItem (item) {           
           this.estado = item.stateId;
           this.titulo = item.title;
           this.idEdit = item.id;
           this.dialog = true
        },

        deleteItem (item) {
            console.log(item);
            this.editedItem = Object.assign({}, item)
            this.dialogDelete = true
        },

        deleteItemConfirm () {
            axios.delete("http://localhost:8000/api/tasks/my/"+this.editedItem.id, { headers: {"Authorization" : `Bearer ${this.access_token}`} }).then((result) => {
                if(result.data.success == 1){
                   this.mostrarSnack('Tarea eliminada con éxito', 'success');
                   this.getTareas();
                }
            })
            this.closeDelete();
        },

      close () {
            this.dialog = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if(this.valid){
            let data = {
                id: this.idEdit,
                state_id: this.estado,
                title: this.titulo,
                comment: this.comentario
            };
            axios.put("http://localhost:8000/api/tasks", data, { headers: {"Authorization" : `Bearer ${this.access_token}`} }).then((result) => {
                if(result.data.success == 1){
                    this.mostrarSnack('Tarea editada con éxito', 'success');
                    this.idEdit = 0;
                    this.estado = 0;
                    this.titulo = '';
                    this.comentario = '';
                    this.getTareas();
                    this.close()
                }
            })
        }        
      },
    }
}
</script>
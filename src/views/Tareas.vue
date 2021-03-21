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
                        class="elevation-4"
                    ></v-data-table>
                </v-col>
            </v-row>
        </v-container>
    </div>
</template>
<script>
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
                { text: 'Fecha', value: 'created_at' },
                { text: 'Acciones', value: 'actions', sortable: false },
            ],
            items:[]
        }
    }
}
</script>
<template>
    <div>
        <v-container fluid>     
            <Header/>       
            <v-row>
                <v-col md="1" sm="1"></v-col>
                <v-col cols="12" md="4" sm="4">
                    <v-card class="elevation-4" >
                        <v-card-title style="background:#ccc" class="mb-2">Compañia</v-card-title>
                        <v-card-text>
                            <v-row>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Nombre" persistent-hint v-model="comp.nombre" hide-details></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Dirección" persistent-hint v-model="comp.direccion" hide-details></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Nit" persistent-hint v-model="comp.nit" hide-details></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Teléfono" persistent-hint v-model="comp.telefono" hide-details></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Correo" persistent-hint v-model="comp.correo" hide-details></v-text-field>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>
                <v-col md="2" sm="2"></v-col>
                <v-col cols="12" md="4" sm="4">
                    <v-card class="elevation-4">
                         <v-card-title style="background:#ccc" class="mb-2">Mi Perfil</v-card-title>
                        <v-card-text>
                            <v-row>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Nombres" persistent-hint v-model="perfil.nombre" hide-details></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Correo" persistent-hint v-model="perfil.correo" hide-details></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field dense outlined disabled label="Se registro el" persistent-hint v-model="perfil.fechaReg" hide-details></v-text-field>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>
                 <v-col md="1" sm="1"></v-col>
            </v-row>            
        </v-container>
    </div>
</template>
<script>
import Header from '../components/Header';
export default {
    name:'Dashboard',
    components:{
        Header
    },
    data(){
        return {
            comp:{
                nombre: '111',
                direccion: '',
                nit: '',
                telefono: '',
                correo: ''
            },
            perfil:{
                nombre: '',
                correo: '',
                fechaReg: ''
            },
            misProyectos:[]
        }
    }
}
</script>
<style scoped>

</style>
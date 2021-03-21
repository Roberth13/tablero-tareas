<template>
    <div>
        <v-container fluid class="mb-2">            
            <v-row class="text-center pt-4">
                <v-col md="1" sm="1" lg="1"></v-col>
                <v-col cols="12" md="4" sm="4" lg="4">
                    <v-form ref="form" v-model="validLogin">
                        <v-row>
                            <v-col cols="12">
                                <h2>Inicia Sesión</h2>
                            </v-col>
                        </v-row>
                         <v-row>
                            <v-col cols="12">
                                <v-text-field hint="Ingrese el correo" dense outlined persistent-hint label="Correo"
                                type="email" v-model="email" :rules="nombreRules" required></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field hint="Ingrese la contraseña" dense outlined persistent-hint label="Contraseña" 
                                type="password" v-model="contrasena"  required :rules="[v => !!v || 'La confirmación es requerida']"></v-text-field>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12" class="text-center">
                                <v-btn @click="login" color="success" large>Ingresar</v-btn>
                            </v-col>
                        </v-row>
                    </v-form>
                </v-col>
                <v-col md="2" sm="2" lg="2">
                    <v-divider vertical></v-divider>
                </v-col>                
                <v-col cols="12" md="4" sm="4" lg="4">
                    <v-form ref="form" v-model="valid">
                        <v-row>
                            <v-col cols="12">
                                <h2>Registrar</h2>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12">
                                <v-text-field hint="Ingrese sus nombres" dense outlined persistent-hint label="Nombres" 
                                                type="text" v-model="nombreReg" :rules="nombreRules" required></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field hint="Ingrese el correo" dense outlined persistent-hint label="Correo" type="email" 
                                                v-model="emailReg" :rules="emailRules" required></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-select hint="Seleccione la compañia" dense outlined persistent-hint label="Compañia"  
                                                v-model="compaReg" :items="companiasItems" :rules="[v => !!v || 'La compañia es requerida']"></v-select>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field hint="Ingrese la contraseña" dense outlined persistent-hint label="Contraseña" type="password" 
                                                v-model="passReg" required :rules="[v => !!v || 'La contraseña es requerida']"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field hint="Confirme la contraseña" dense outlined persistent-hint label="Confirmación" type="password" 
                                                v-model="passConfReg" required :rules="[v => !!v || 'La confirmación es requerida']"></v-text-field>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12" class="text-center">
                                <v-btn :disabled="!valid" color="success" large>Registrarme</v-btn>
                            </v-col>
                        </v-row>
                    </v-form>
                </v-col>
                <v-col md="2" sm="2" lg="2"></v-col>
            </v-row>
        </v-container>
    </div>
</template>
<script>
export default {
    name: 'Login',
    data(){
        return{
            valid: false,
            validLogin: false,
            email: '',
            contrasena: '',
            companiasItems:[],
            nombreReg:'',
            emailReg:'',
            compaReg: null,
            passReg:'',
            passConfReg:'',
            nombreRules: [
                v => !!v || 'Los nombres son requeridos'
            ],
             emailRules: [
                v => !!v || 'El correo es requerido',
                v => /.+@.+\..+/.test(v) || 'Ingrese un correo valido',
            ],
        }
    },
    methods:{
        login(){
            this.$router.push({
                path: 'dashboard'
            })
        }
    }
}
</script>
<style scoped>

</style>
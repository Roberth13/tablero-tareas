<template>
    <div>
        <v-container fluid class="mb-2">            
            <v-row class="text-center pt-4">
                <v-col md="1" sm="1" lg="1"></v-col>
                <v-col cols="12" md="4" sm="4" lg="4">
                    <v-form ref="formLog" v-model="validLogin">
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
                                <v-btn :disabled="!validLogin" @click="login" color="success" large>Ingresar</v-btn>
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
                                                v-model="compaReg" :items="companiasItems" :rules="[v => !!v || 'La compañia es requerida']"
                                                item-text="names" item-value="id"></v-select>
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
                                <v-btn :disabled="!valid" color="success" @click="crear" large>Registrarme</v-btn>
                            </v-col>
                        </v-row>
                    </v-form>
                </v-col>
                <v-col md="2" sm="2" lg="2"></v-col>
            </v-row>
        </v-container>
        <v-snackbar v-model="snack" :color="color_snack">{{ mensaje }}</v-snackbar>
    </div>
</template>
<script>
import axios from "axios";
export default {
    name: 'Login',
    data(){
        return{
            color_snack: 'success',
            mensaje: '',
            snack: false,
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
            if(this.validLogin){
                let data = {
                    email: this.email,
                    password: this.contrasena
                }
                axios.post("http://localhost:8000/api/login", data).then((result) => {
                    if(result.data.access_token.length > 0){
                        localStorage.setItem('access_token', JSON.stringify(result.data.access_token))
                        this.$router.push({
                            path: 'dashboard'
                        });
                        this.$refs.formLog.reset();
                        this.$refs.formLog.resetValidation();
                    }
                }) 
            }
            
        },
        crear(){
            if(this.valid){
                let data = {
                    name: this.nombreReg,
                    password: this.passReg,
                    password_confirmation: this.passConfReg,
                    company_id: this.compaReg,
                    email: this.emailReg
                }
                axios.post("http://localhost:8000/api/register", data).then((result) => {
                    if(result.data.access_token.length > 0){
                        this.mensaje = 'Usuario registrado con éxito';
                        this.snack = true;
                        this.color_snack = 'success';
                        setTimeout(() =>{
                            this.snack = false;
                        }, 3000);
                        this.email = this.emailReg;
                        this.$refs.form.reset();
                        this.$refs.form.resetValidation();
                    }
                }) 
                
            }
        }
    },
    created() {
        axios.get("http://localhost:8000/api/companies").then((result) => {
            if(result.data.success == 1){
                this.companiasItems = result.data.data;
            }
        })
    }
}
</script>
<style scoped>

</style>
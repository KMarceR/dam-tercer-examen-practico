<template>
    <ion-page>
        <ion-header>
            <ion-toolbar>
                <ion-title>Login</ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col>
                        <ion-input label="Correo" label-placement="stacked" fill="outline" shape="round"
                            clear-input="true" placeholder="Correo electrónico" v-model="usuario.email"></ion-input>
                        <br>
                        <ion-input label="Contraseña" label-placement="stacked" fill="outline" shape="round"
                            clear-input="true" placeholder="***********" type="password"
                            v-model="usuario.password"></ion-input>
                        <br>
                        <ion-button shape="round" color="primary" expand="block" @click="login">
                            Iniciar sesión
                        </ion-button>
                    </ion-col>
                </ion-row>
            </ion-grid>
            <!-- Toast -->
            <ion-toast :duration="2500" :message="toastMessage" :is-open="toastState" @didDismiss="toastState = false"
                :icon="informationCircleOutline"></ion-toast>
        </ion-content>
    </ion-page>
</template>

<script>
// Importación de iconos 
import { informationCircleOutline } from 'ionicons/icons'
// Importación de componentes Ionic 
import {
    IonPage, IonContent, IonGrid, IonRow, IonCol, IonIcon,
    IonHeader, IonToolbar, IonTitle, IonButton, IonInput, IonToast
} from '@ionic/vue'
// Importación de axios 
import axios from 'axios'

export default {
    name: 'Login',
    components: {
        IonPage, IonContent, IonGrid, IonRow, IonCol, IonIcon,
        IonHeader, IonToolbar, IonTitle, IonButton, IonInput, IonToast
    },
    data() {
        return {
            // Icono 
            informationCircleOutline,
            // Variable para guardar el contenido del formulario 
            usuario: {
                email: null,
                password: null
            },
            // Variable para controlar la visibilidad del toast 
            toastState: false,
            // Variable para guardar el mensaje a mostrar en el toast 
            toastMessage: null
        }
    },
    methods: {
        login() {
            // Petición para realizar login 
            axios.post('http://127.0.0.1:8000/api/login', this.usuario)
                .then(response => {
                    // console.log(response) 
                    if (response.data.code == 200) {
                        // Si se valida el acceso:  
                        // Se guarda el token en el Ionic Storage 
                        this.$storage.set('token', response.data.token)
                        // Se guarda el usuario en el Ionic Storage 
                        this.$storage.set('user', response.data.data.name)
                        // Se redirecciona a la primera pantalla de la aplicación 
                        this.$router.push('/tabs/addCliente')
                    }
                })
                .catch(error => {
                    this.toastState = true
                    if (error.response.code == 401) {
                        // Usuario no autorizado 
                        this.toastMessage = error.response.data.data
                    } else {
                        // Otros errores 
                        this.toastMessage = 'Ha ocurrido un error'
                    }
                })
        }
    }
} 
</script>
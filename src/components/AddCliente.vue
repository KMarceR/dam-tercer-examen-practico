<template>
    <ion-page>
        <ToolBar title="Nuevo Cliente"></ToolBar>
        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col>
                        <ion-input label="Nombre" color="warning" label-placement="stacked" maxlength="30" counter
                            fill="outline" shape="round" clear-input="true" placeholder="Nombres y apellidos"
                            v-model="cliente.nombre_cliente"></ion-input>
                        <br>
                        <ion-input label="Edad" color="warning" label-placement="stacked" fill="outline" shape="round"
                            clear-input="true" placeholder="Edad" type="number" v-model="cliente.edad_cliente"></ion-input>
                        <br>
                        <ion-input label="Categoria" color="warning" label-placement="stacked" maxlength="9" counter
                            fill="outline" shape="round" clear-input="true" placeholder="Categoria"
                            v-model="cliente.codigo_categoria"></ion-input>
                        <br>
                        <ion-button shape="round" color="warning" expand="block" @click="addCliente">
                            <ion-icon slot="start" :icon="checkmarkOutline"> </ion-icon>
                            Guardar
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
import { checkmarkOutline, informationCircleOutline } from 'ionicons/icons'
import {
    IonPage, IonContent, IonGrid, IonRow, IonCol, IonIcon,
    IonHeader, IonToolbar, IonTitle, IonButton, IonInput, IonToast
} from '@ionic/vue'

import ToolBar from '@/components/ToolBar.vue' 

// Importando axios 
import axios from 'axios'

export default {
    name: 'AddCliente',
    components: {
        IonPage, IonContent, IonGrid, IonRow, IonCol, IonIcon,
        IonHeader, IonToolbar, IonTitle, IonButton, IonInput, IonToast, ToolBar
    },
    data() {
        return {
            checkmarkOutline, informationCircleOutline,
            cliente: {},
            toastState: false,
            toastMessage: null,
            // Variable para definir el header de autorización 
            config: { },
        }
    },
    methods: {
        addCliente() {
            // Petición para insertar datos 
            axios.post('http://127.0.0.1:8000/api/clientes/store', this.cliente,this.config)
                .then(response => {
                    let res = response.data
                    this.cliente = {}
                    if (res.code == 200) {
                        this.toastState = true
                        this.toastMessage = res.data
                    }
                })
                .catch(error => console.log('Ha ocurrido un error'))
        },
        async getToken() {
            let token = await this.$storage.get('token')
            this.config = {
                headers: {
                    'Authorization': 'Bearer ' + token
                }
            }
        },
    },
    ionViewWillEnter() {
        this.getToken() 
    }
} 
</script>
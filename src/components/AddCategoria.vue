<template>
    <ion-page>
        <ToolBar title="Nueva Categoria"></ToolBar>
        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col>
                        <ion-input label="Nombre" color="warning" label-placement="stacked" maxlength="30" counter
                            fill="outline" shape="round" clear-input="true" placeholder="Nombres de la categoria"
                            v-model="categoria.nombre_categoria"></ion-input>
                        <br>
                        <ion-button shape="round" color="warning" expand="block" @click="addCategoria">
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
    name: 'AddCategoria',
    components: {
        IonPage, IonContent, IonGrid, IonRow, IonCol, IonIcon,
        IonHeader, IonToolbar, IonTitle, IonButton, IonInput, IonToast, ToolBar
    },
    data() {
        return {
            checkmarkOutline, informationCircleOutline,
            categoria: {},
            toastState: false,
            toastMessage: null,
            // Variable para definir el header de autorización 
            config: {},
        }
    },
    methods: {
        addCategoria() {
            // Petición para insertar datos 
            axios.post('http://127.0.0.1:8000/api/categorias/store', this.categoria,this.config)
                .then(response => {
                    let res = response.data
                    this.categoria = {}
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
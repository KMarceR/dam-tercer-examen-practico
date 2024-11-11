<template>
    <ion-page>
        <ToolBar title="Categorias"></ToolBar>
        <ion-content class="ion-padding">
            <ion-grid>
                <ion-row>
                    <ion-col>
                        <ion-card>
                            <ion-card-header>
                                <ion-card-title>
                                    Categorias
                                </ion-card-title>
                                <ion-card-subtitle>
                                    Listado de Categorias
                                </ion-card-subtitle>
                            </ion-card-header>
                            <ion-card-content>
                                <ion-list>
                                    <template v-for="(categoria, i) in respuesta" :key="i">
                                        <ion-item-sliding>
                                            <ion-item :button="true">
                                                <ion-icon slot="start" :icon="shapesOutline"></ion-icon>
                                                <ion-label>
                                                    {{ categoria.nombre_categoria }}
                                                </ion-label>
                                            </ion-item>
                                            <ion-item-options slot="end">
                                                <ion-item-option color="tertiary"
                                                    @click="showCategoria(categoria.codigo_categoria, 1)">
                                                    <ion-icon slot="icon-only" :icon="eye"></ion-icon>
                                                </ion-item-option>
                                                <ion-item-option color="success"
                                                    @click="showCategoria(categoria.codigo_categoria, 2)">
                                                    <ion-icon slot="icon-only" :icon="create"></ion-icon>
                                                </ion-item-option>
                                                <ion-item-option color="danger"
                                                    @click="deleteCategoria(categoria.codigo_categoria)">
                                                    <ion-icon slot="icon-only" :icon="trash"></ion-icon>
                                                </ion-item-option>
                                            </ion-item-options>
                                        </ion-item-sliding>
                                    </template>

                                    <ion-item v-if="respuesta.length == 0">
                                        <ion-icon slot="start" :icon="closeCircle"></ion-icon>
                                        <ion-label>No hay registros</ion-label>
                                    </ion-item>
                                </ion-list>
                            </ion-card-content>
                        </ion-card>
                    </ion-col>
                </ion-row>
            </ion-grid>
        </ion-content>



        <ion-modal :is-open="modalState1" @didDismiss="modalState1 = false">
            <ion-header>
                <ion-toolbar>
                    <ion-title>Información de la categoria</ion-title>
                    <ion-buttons slot="end">
                        <ion-button @click="modalState1 = false">
                            <ion-icon slot="icon-only" :icon="closeCircle"></ion-icon>
                        </ion-button>
                    </ion-buttons>
                </ion-toolbar>
            </ion-header>
            <ion-content class="ion-padding">
                <ion-list>
                    <ion-item>
                        <ion-icon slot="start" :icon="shapesOutline"></ion-icon>
                        <ion-label>{{ categoria.nombre_categoria }}</ion-label>
                    </ion-item>
                </ion-list>
            </ion-content>
        </ion-modal>

        <ion-modal :is-open="modalState2" @didDismiss="modalState2 = false">
            <ion-header>
                <ion-toolbar>
                    <ion-title>Información de la categoria</ion-title>
                    <ion-buttons slot="end">
                        <ion-button @click="modalState2 = false">
                            <ion-icon slot="icon-only" :icon="closeCircle"></ion-icon>
                        </ion-button>
                    </ion-buttons>
                </ion-toolbar>
            </ion-header>
            <ion-content class="ion-padding">
                <ion-input label="Nombre" color="warning" label-placement="stacked" maxlength="30" counter
                    fill="outline" shape="round" clear-input="true" placeholder="Nombre de la categoria"
                    v-model="categoria.nombre_categoria"></ion-input>
                <br>
                <ion-button shape="round" color="warning" expand="block"
                    @click="editCategoria(categoria.codigo_categoria)">
                    <ion-icon slot="start" :icon="checkmarkOutline"></ion-icon>
                    Guardar cambios
                </ion-button>
            </ion-content>
        </ion-modal>

        <ion-toast :duration="2500" :message="toastMessage" :is-open="toastState" @didDismiss="toastState = false"
            :icon="informationCircleOutline"></ion-toast>

    </ion-page>
</template>
<script>
import {
    eye, trash, create, closeCircle, call, mail, shapesOutline,
    checkmark, checkmarkOutline, informationCircleOutline
} from 'ionicons/icons'
import {
    IonPage, IonContent, IonGrid, IonRow, IonCol, IonList, IonItem, IonIcon,
    IonLabel, IonHeader, IonToolbar, IonTitle, IonCard, IonCardContent,
    IonCardHeader, IonCardTitle, IonCardSubtitle, IonInput, IonItemSliding,
    IonItemOption, IonItemOptions
} from '@ionic/vue'

import ToolBar from '@/components/ToolBar.vue'

import axios from 'axios'

export default {
    name: 'Categoria',
    components: {
        IonPage, IonContent, IonGrid, IonRow, IonCol, IonList, IonItem, IonIcon,
        IonLabel, IonHeader, IonToolbar, IonTitle, IonCard, IonCardContent,
        IonCardHeader, IonCardTitle, IonCardSubtitle, IonInput, IonItemSliding,
        IonItemOption, IonItemOptions, ToolBar
    },
    data() {
        return {
            eye, trash, create, closeCircle, call, mail, shapesOutline,
            checkmark, checkmarkOutline, informationCircleOutline, respuesta: [],
            modalState1: false, categoria: {}, modalState2: false, toastState: false, toastMessage: null,
            // Variable para definir el header de autorización 
            config: {},
        }
    },
    methods: {
        // Petición para consultar datos 
        loadData() {
            this.respuesta = []
            axios.get('http://127.0.0.1:8000/api/categorias/select',this.config)
                .then(response => {
                    let res = response.data
                    if (res.code == 200) {
                        this.respuesta = res.data
                    }
                })
                .catch(error => console.log('Ha ocurrido un error'))
        },
        showCategoria(id, action) {
            axios.get(`http://127.0.0.1:8000/api/categorias/find/${id}`,this.config)
                .then(response => {
                    let res = response.data
                    if (res.code == 200) {
                        this.categoria = res.data
                    }
                })
                .catch(error => console.log('Ha ocurrido un error'))
            if (action == 1) {
                this.modalState1 = true
            } else {
                this.modalState2 = true
            }
        },
        editCategoria(id) {
            axios.put(`http://127.0.0.1:8000/api/categorias/update/${id}`, this.categoria)
                .then(response => {
                    let res = response.data
                    if (res.code == 200) {
                        this.toastState = true
                        this.toastMessage = res.data
                        this.modalState2 = false
                        this.loadData()
                    }
                })
                .catch(error => console.log('Ha ocurrido un error'))
        },
        deleteCategoria(id) {
            axios.delete(`http://127.0.0.1:8000/api/categorias/delete/${id}`,this.config)
                .then(response => {
                    let res = response.data
                    if (res.code == 200) {
                        this.toastState = true
                        this.toastMessage = res.data
                        this.loadData()
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
    async ionViewWillEnter() {
        await this.getToken();
        this.loadData();
    }
} 
</script>
<template>
    <ion-header>
        <ion-toolbar>
            <ion-title>{{ title }}</ion-title>
            <ion-buttons slot="end">
                <ion-button @click="logout" icon="exitOutline">Cerrar</ion-button>
            </ion-buttons>
        </ion-toolbar>
    </ion-header>
</template>

<script>
import { IonHeader, IonToolbar, IonTitle, IonButtons, IonButton } from '@ionic/vue'
import { exitOutline } from 'ionicons/icons'

export default {
    name: 'ToolBar',
    components: {
        IonHeader, IonToolbar, IonTitle, IonButtons, IonButton
    },
    props: {
        title: String
    },
    data() {
        return {
            exitOutline
        }
    },
    methods: {
        // Método para cerrar sesión 
        async logout() {
            // Se limpia el storage 
            await this.$storage.clear()

            // Se redirecciona al login 
            this.$router.push('/')
        }
    }
}
</script>
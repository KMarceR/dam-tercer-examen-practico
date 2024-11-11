<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="tab1" href="/tabs/addCliente">
          <ion-icon aria-hidden="true" :icon="addCircleOutline" />
          <ion-label>Agregar Cliente</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="tab2" href="/tabs/Clientes">
          <ion-icon aria-hidden="true" :icon="personCircleOutline" />
          <ion-label>Cliente</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="tab3" href="/tabs/addCategoria">
          <ion-icon aria-hidden="true" :icon="addCircleOutline" />
          <ion-label>Agregar Categria</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="tab4" href="/tabs/Categorias">
          <ion-icon aria-hidden="true" :icon="shapesOutline" />
          <ion-label>Categoria</ion-label>
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script setup>
import { IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, IonPage, IonRouterOutlet }
  from '@ionic/vue';
import { personCircleOutline, addCircleOutline,shapesOutline } from 'ionicons/icons'; 
</script>
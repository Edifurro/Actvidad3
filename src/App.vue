<template>
  <ion-app>
    <ion-menu content-id="main-content">
      <ion-header>
        <ion-toolbar>
          <ion-title>Menu</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <ion-list>
          <ion-item button @click="navigateTo('/home')">
            <ion-label>Home</ion-label>
          </ion-item>
          <ion-item button @click="navigateTo('/page1')">
            <ion-label>gordas</ion-label>
          </ion-item>
          <ion-item button @click="navigateTo('/page2')">
            <ion-label>Indice de masa</ion-label>
          </ion-item>
          <ion-item button @click="navigateTo('/page3')">
            <ion-label>Cuanto le debes al sat</ion-label>
          </ion-item>
        </ion-list>
      </ion-content>
    </ion-menu>
    <ion-router-outlet id="main-content"></ion-router-outlet>
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonContent, IonHeader, IonItem, IonLabel, IonList, IonMenu, IonRouterOutlet, IonTitle, IonToolbar, menuController } from '@ionic/vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const navigateTo = async (url: string) => {
  await router.push(url);
  menuController.close(); // Close the menu after navigation
};
</script>

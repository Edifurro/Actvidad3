<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>Angel te chingaria?</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <h1>Introduce tu peso</h1>

      <ion-item>
        <ion-input
          label="Peso (kg)"
          label-placement="floating"
          type="number"
          placeholder="Introduce tu peso en kilogramos (e.g., 70)"
          v-model="weight"
        ></ion-input>
      </ion-item>

      <div v-if="weight && parseFloat(weight) > 100" style="margin-top: 20px; text-align: center;">
        <img src="../assets/wow.jpg" alt="Angel" />
        <p>Escondete angel te va a culiar!</p>
      </div>

    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonInput, IonItem } from '@ionic/vue';

const weight = ref<string | null>(null); // Use string or null for v-model with input, parse to number for comparison

</script>

<style scoped>
/* Add any specific styles for this page here */
ion-item {
  margin-bottom: 16px;
}
</style>
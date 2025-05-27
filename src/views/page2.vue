<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>IMC Calculadora</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <h1>Calcula tu índice de masa corporal</h1>

      <ion-item>
        <ion-label position="floating" style="margin-bottom: 20px;" >peso (kg)</ion-label>
        <ion-input
          type="number"
        
          placeholder="Ingresa peso en kilogramos (e.g., 70)"
          v-model="weight"
        ></ion-input>
      </ion-item>

      <ion-item>
        <ion-label position="floating" style="margin-bottom: 20px;">Altura (m)</ion-label>
        <ion-input
          type="number"
          step="0.01"
          placeholder="Ingresa altura en metros (e.g., 1.75)"
          v-model="height"
        ></ion-input>
      </ion-item>

      <ion-button expand="block" @click="calculateBmi" style="margin-top: 20px;">Calcular</ion-button>

      <div v-if="bmi !== null" style="margin-top: 20px; text-align: center;">
        <h2>Tu indice de masa corporal es: {{ bmi.toFixed(2) }}</h2>
        <p>{{ bmiCategory }}</p>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import {
  IonButtons,
  IonContent,
  IonHeader,
  IonMenuButton,
  IonPage,
  IonTitle,
  IonToolbar,
  IonInput,
  IonItem,
  IonButton,
  IonLabel // Added IonLabel here
} from '@ionic/vue';

const weight = ref<string | null>(null);
const height = ref<string | null>(null);
const bmi = ref<number | null>(null);

const calculateBmi = () => {
  const w = parseFloat(weight.value || '0');
  const h = parseFloat(height.value || '0');

  if (w > 0 && h > 0) {
    bmi.value = w / (h * h);
  } else {
    bmi.value = null; // Reset or indicate invalid input
  }
};

const bmiCategory = computed(() => {
  if (bmi.value === null) return '';
  if (bmi.value < 18.5) return 'Desnutrición';
  if (bmi.value < 24.9) return 'Peso normal';
  if (bmi.value < 29.9) return 'Sobrepeso';
  return 'Obesidad';
});

</script>

<style scoped>
ion-item {
  margin-bottom: 18px;
}
h2 {
  font-weight: bold;
}
:placeholder-shown {
  color: #8c8c8c; /* Placeholder text color */
  
  
}

</style>
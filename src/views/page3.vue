<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>Calculadora ISR(Mexico - Mensual)</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <h1>Calcular el ISR mensual</h1>
      <p style="font-size: 0.8em; color: #888;">Nota: Esta calculadora utiliza una tabla simplificada de impuestos de ejemplo con fines ilustrativos. Consulte siempre las tablas oficiales del SAT para realizar cálculos precisos.</p>

      <ion-item>
        <ion-label position="floating" style="margin-bottom: 20px;">Ingresos brutos mensuales (MXN)</ion-label>
        <ion-input
          type="number"
          placeholder="Ingresa el ingreso bruto mensual"
          v-model="grossIncome"
        ></ion-input>
      </ion-item>

      <ion-button expand="block" @click="calculateIsr" style="margin-top: 20px;">Calcular isr</ion-button>

      <div v-if="isrAmount !== null && !calculationError" style="margin-top: 20px; text-align: center;">
        <h2>ISR a pagar${{ isrAmount.toFixed(2) }} MXN</h2>
        <p v-if="applicableBracket">
           Soporte aplicable:<br/>
          Limite Inferior: ${{ applicableBracket.lowerLimit.toFixed(2) }} <br/>
          Limite Superior: {{ applicableBracket.upperLimit === Infinity ? 'En adelante' : '$' + applicableBracket.upperLimit.toFixed(2) }} <br/>
           Cuota fija: ${{ applicableBracket.fixedFee.toFixed(2) }} <br/>
           Porcentaje sobre el excedente:{{ (applicableBracket.percentage * 100).toFixed(2) }}%
        </p>
      </div>
      <div v-if="calculationError" style="margin-top: 20px; text-align: center; color: red;">
        <p>{{ calculationError }}</p>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
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
  IonLabel
} from '@ionic/vue';

interface TaxBracket {
  lowerLimit: number;
  upperLimit: number;
  fixedFee: number;
  percentage: number; // Store as decimal, e.g., 0.0192 for 1.92%
}

// Example Monthly ISR Table (Illustrative - based on typical structures, verify with official SAT data)
// This table should be updated with current official data for real use.
const monthlyIsrTable: TaxBracket[] = [
  { lowerLimit: 0.01,     upperLimit: 746.04,    fixedFee: 0.00,    percentage: 0.0192 },
  { lowerLimit: 746.05,    upperLimit: 6332.05,   fixedFee: 14.32,   percentage: 0.0640 },
  { lowerLimit: 6332.06,   upperLimit: 11128.01,  fixedFee: 371.83,  percentage: 0.1088 },
  { lowerLimit: 11128.02,  upperLimit: 12935.82,  fixedFee: 893.63,  percentage: 0.1600 },
  { lowerLimit: 12935.83,  upperLimit: 15487.71,  fixedFee: 1182.88, percentage: 0.1792 },
  { lowerLimit: 15487.72,  upperLimit: 31236.49,  fixedFee: 1640.18, percentage: 0.2136 },
  { lowerLimit: 31236.50,  upperLimit: 49233.00,  fixedFee: 4999.59, percentage: 0.2352 },
  { lowerLimit: 49233.01,  upperLimit: 93993.90,  fixedFee: 9236.89, percentage: 0.3000 },
  { lowerLimit: 93993.91,  upperLimit: 125325.20, fixedFee: 22665.17,percentage: 0.3200 },
  { lowerLimit: 125325.21, upperLimit: 375975.61, fixedFee: 32691.18,percentage: 0.3400 },
  { lowerLimit: 375975.62, upperLimit: Infinity,  fixedFee: 117705.32,percentage: 0.3500 },
];

const grossIncome = ref<string | null>(null);
const isrAmount = ref<number | null>(null);
const applicableBracket = ref<TaxBracket | null>(null);
const calculationError = ref<string | null>(null);

const calculateIsr = () => {
  isrAmount.value = null;
  applicableBracket.value = null;
  calculationError.value = null;

  const income = parseFloat(grossIncome.value || '0');

  if (isNaN(income) || income < 0) {
    calculationError.value = 'Please enter a valid positive income.';
    return;
  }
  if (income === 0) {
     isrAmount.value = 0;
     calculationError.value = 'Income is zero, ISR is zero.'; // Or handle as per specific rules for zero income
     return;
  }


  let foundBracket: TaxBracket | null = null;

  for (const bracket of monthlyIsrTable) {
    if (income >= bracket.lowerLimit && income <= bracket.upperLimit) {
      foundBracket = bracket;
      break;
    }
  }

  if (foundBracket) {
    applicableBracket.value = foundBracket;
    const surplus = income - foundBracket.lowerLimit;
    const taxOnSurplus = surplus * foundBracket.percentage;
    isrAmount.value = foundBracket.fixedFee + taxOnSurplus;
  } else {
    // This case should ideally not happen if the table covers all ranges (especially with Infinity)
    // but as a fallback:
    calculationError.value = 'Could not determine the applicable tax bracket. Please check the income or tax table.';
  }
};

</script>

<style scoped>
ion-item {
  margin-bottom: 16px;
}
h2 {
  font-weight: bold;
}
p {
  line-height: 1.4;
}
</style>
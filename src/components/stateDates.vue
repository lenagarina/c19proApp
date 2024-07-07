<template><ion-card no-body class="cpro_card-m">
  <ion-row class="textblock-style">
    <ion-col lg="12">
      <h2>Corona-Zahlen</h2>
      <h4 v-if="scope.statesStatistics !== null" class="m-0">
        Hier finden Sie aktuelle COVID-19 Kennzahlen für Bundesland
        {{ scope.statesStatistics.name }}
      </h4>
    </ion-col>
  </ion-row>
  <ion-row class="ion-margin-top" v-if="scope.isLoading">
    <ion-col lg="4" md="6" size-sm="12">
      <ion-spinner></ion-spinner>
    </ion-col>
    <ion-col lg="4" md="6" size-sm="12">
      <ion-spinner></ion-spinner>
    </ion-col>
    <ion-col lg="4" md="6" size-sm="12">
      <ion-spinner></ion-spinner>
    </ion-col>
  </ion-row>

  <ion-row v-if="scope.statesStatistics !== null && !scope.isLoading">
    <ion-col size-lg="4" size-md="6" size-sm="12">
      <div class="cpro_cell">
        <h1>{{ scope.statesStatistics.population.toLocaleString() }}</h1>
        <span>Einwohner
          <ion-icon :icon="informationCircleOutline" id="hover-trigger-1"></ion-icon>
          <ion-popover trigger="hover-trigger-1" trigger-action="hover">
            <ion-content class="ion-padding">Einwohnerzahl nach statistischem Landesamt</ion-content>
          </ion-popover>


        </span>
      </div>
    </ion-col>
    <ion-col size-lg="4" size-md="6" size-sm="12">
      <div class="cpro_cell">
        <h1>{{ scope.statesStatistics.cases.toLocaleString() }}</h1>
        <span>Infektionen (gesamt)

          <ion-icon :icon="informationCircleOutline" id="hover-trigger-2"></ion-icon>
          <ion-popover trigger="hover-trigger-2" trigger-action="hover">
            <ion-content class="ion-padding">Gesamtzahl aller gemeldeten</ion-content>
          </ion-popover>
        </span>
      </div>
    </ion-col>
    <ion-col size-lg="4" size-md="6" size-sm="12">
      <div class="cpro_cell">
        <h1>{{ scope.statesStatistics.deaths.toLocaleString() }}</h1>
        <span>Todesfälle (gesamt)
          <ion-icon :icon="informationCircleOutline" id="hover-trigger-3"></ion-icon>
          <ion-popover trigger="hover-trigger-3" trigger-action="hover">
            <ion-content class="ion-padding">Gesamtzahl aller gemeldeten</ion-content>
          </ion-popover>
        </span>
      </div>
    </ion-col>
  </ion-row>
</ion-card></template>

<script setup lang="ts">
import { reactive, onMounted } from 'vue'
import axios from 'axios'
import { useRoute } from 'vue-router'
import {
  IonCol, IonRow, IonPopover, IonContent, IonIcon,
  IonCard, IonSpinner
} from '@ionic/vue';
import { informationCircleOutline } from 'ionicons/icons';
import { type StatesStatisticsType } from '../components/types/dataTypes'

const props = defineProps({
  type: { type: String, required: false }
})

const route = useRoute()

const scope: {
  statesStatistics: StatesStatisticsType | null
  isLoading: boolean
} = reactive({
  statesStatistics: null,
  isLoading: false
})

function getURLIdent(): string {
  if (props.type === 'district') {
    return route?.params?.ags.toString() ?? ''
  }
  if (props.type === 'state') {
    return route?.params?.abbreviation.toString() ?? ''
  }
  return ''
}

function getURLAPI(): string {
  if (props.type === 'district') {
    return 'districts'
  }
  if (props.type === 'state') {
    return 'states'
  }
  return ''
}

async function fetchStatesStatistics() {
  scope.isLoading = true
  axios
    .get('https://api.corona-zahlen.org/' + getURLAPI() + '/' + getURLIdent())
    .then((response) => {
      const responseData = response.data.data[getURLIdent()]
      scope.statesStatistics = responseData
      scope.isLoading = false
    })
}

onMounted(() => {
  fetchStatesStatistics()
})
</script>

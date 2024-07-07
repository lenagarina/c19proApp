<template><ion-card no-body class="cpro_card-m">
  <ion-row>
    <ion-col lg="12">
      <div class="cpro_bg-primary text-light ion-padding">
        <h2>Statistik zu COVID-19 Infektionen nach Bundesländer</h2>
        <h3>
          Hier finden Sie aktuelle COVID-19 Kennzahlen für alle 16 Bundesländer in Deutschland
        </h3>
      </div>

      <div class="cpro_alert alert alert-primary alert-dismissible fade show" role="alert" v-if="scope.isAlertVisible">

        <ion-icon class="close alert-close" @click="scope.isAlertVisible = false" :icon="closeOutline"
          size="large"></ion-icon>

        <h5>
          <strong>Hinweis:</strong>
          Nutzen Sie die Suchfunktion, um Kennzahlen für Ihr Bundesland schneller zu finden oder
          klicken Sie auf eine Spalte, um die Tabelle neu zu sortieren. Mit Klick auf den Namen
          eines Bundeslands erhalten Sie weitere Informationen.
        </h5>
      </div>
      <div class="cpro_table-container">
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">Bundesland</th>
              <th scope="col">Befölkerung</th>
              <th scope="col">Infektionen</th>
              <th scope="col">Todesfälle</th>
              <th scope="col">Erholt</th>
              <th scope="col">Aktualisiert</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in scope.statesStatisticsArray
        .filter(filterFunction)
        .sort(compareFunction)
        .slice(0, 7 * scope.currentPage)" :key="index">
              <td>
                <RouterLink :to="'/state/' + item.abbreviation">
                  <div>{{ item.name }}</div>
                </RouterLink>

              </td>
              <td>{{ item.population.toLocaleString() }}</td>
              <td>
                <div class="progress cpro_progress-bar">
                  <div class="progress-bar" role="progressbar"
                    :style="{ width: Math.round(percentCalculater(item) ?? 0) + '%' }"
                    :aria-valuenow="Math.round(percentCalculater(item) ?? 0)" aria-valuemin="0" aria-valuemax="100">
                    <div class="percentCalculaterNumber">{{ percentCalculater(item) }} %</div>
                  </div>
                </div>
              </td>
              <td>{{ item.deaths.toLocaleString() }}</td>
              <td>{{ item.recovered.toLocaleString() }}</td>
              <td>{{ item.hospitalization.lastUpdate.substring(0, 10).replace('-', '.') }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </ion-col>
  </ion-row>
  <ion-row>
    <ion-col lg="4" md="6">
      <div class="input-group has-validation ms-4">

        <ion-searchbar id="validationCustomUsername" placeholder="Welche Bundesland?"
          v-model="scope.filterName"></ion-searchbar>

      </div>
    </ion-col>
    <ion-col lg="8" md="6">

      <ion-button v-if="scope.currentPage * 7 < scope.statesStatisticsArray.length"
        @click="ionInfinite">Weiter</ion-button>

    </ion-col>
  </ion-row>
</ion-card></template>

<script setup lang="ts">
import { reactive, onMounted } from 'vue'
import axios from 'axios'
import {
  IonIcon, IonCol, IonRow, InfiniteScrollCustomEvent,
  IonCard, IonSearchbar, IonButton
} from '@ionic/vue';
import { closeOutline } from 'ionicons/icons';
import { type MetaInformation, type StatesStatisticsType } from './types/dataTypes'

const scope: {
  filterName: string
  statesStatisticsArray: Array<StatesStatisticsType>
  metaInformation: MetaInformation | null
  currentPage: number
  toastVisible: boolean
  isAlertVisible: boolean
} = reactive({
  filterName: '',
  statesStatisticsArray: [],
  metaInformation: null,
  currentPage: 1,
  toastVisible: true,
  isAlertVisible: true
})

async function fetchStatesStatistics() {
  axios.get('https://api.corona-zahlen.org/states').then((response) => {
    const responseData = response.data.data
    scope.metaInformation = response.data.meta
    const allValues: Array<StatesStatisticsType> = Object.values(responseData)
    scope.statesStatisticsArray = allValues
  })
}

const ionInfinite = (ev: InfiniteScrollCustomEvent) => {

  scope.currentPage++;
};



// arg item: StatesStatisticsType - object element with states statistic data
// return the result of an equation with numbers
const percentCalculater = (item: StatesStatisticsType): number | null => {
  if (
    !Number.isInteger(item.cases) ||
    !Number.isInteger(item.population) ||
    item.population === 0
  ) {
    return null
  }

  const result = (item.cases / item.population) * 100
  return Math.round(result * 10) / 10
}

// arg item: StatesStatisticsType - object element with statistic data
// return boolean - check substring filterName in item.name
const filterFunction = (item: StatesStatisticsType): boolean => {
  return item.name.toLowerCase().includes(scope.filterName.toLowerCase())
}

// function compare 2 arguments
// sort function returns a type number (-1, 0, 1)
const compareFunction = (a: StatesStatisticsType, b: StatesStatisticsType): number => {
  if (a.name > b.name) {
    return 1
  } else if (a.name < b.name) {
    return -1
  } else {
    return 0
  }
}

onMounted(() => {
  fetchStatesStatistics()
})
</script>

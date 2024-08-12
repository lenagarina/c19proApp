<template><ion-card no-body class="cpro_card-m">
  <ion-row>
    <ion-col lg="12">

      <div class="cpro_bg-primary text-light ion-padding">
        <h2>Corona-Zahlen für Landkreise in Deutschland</h2>
        <h3>
          Hier finden Sie aktuelle COVID-19 Kennzahlen für alle Landkreise und kreisfreien Städte
          <template v-if="props.stateAbbreviation !== undefined">(Bundesland: {{ props.stateAbbreviation }})</template>
        </h3>
      </div>

      <div class="cpro_alert alert alert-primary alert-dismissible fade show" role="alert" v-if="scope.isAlertVisible">
        <ion-icon class="close alert-close" @click="scope.isAlertVisible = false" :icon="closeOutline"
          size="large"></ion-icon>
        <h5>
          <strong>Hinweis:</strong>
          Nutzen Sie die Suchfunktion, um Kennzahlen für Ihr Landkreis schneller zu finden oder
          klicken Sie auf eine Spalte, um die Tabelle neu zu sortieren. Mit dem Klick auf den Namen
          eines Landkreises erhalten Sie weitere Informationen.
        </h5>
      </div>
      <div class="cpro_table-container">
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col-2">
                Landkreis <br />
                (Bundesland)
              </th>
              <th scope="col-2">
                Befölkerung <br />
                des Landkreises
              </th>
              <th scope="col-2">
                Infektionen <br />
                (gesamt)
              </th>
              <th scope="col-2">
                Todesfälle <br />
                (gesamt)
              </th>
              <th scope="col-1">
                Infektionen <br />
                pro Woche
              </th>
              <th scope="col-1">
                Todesfälle <br />
                pro Woche
              </th>
              <th scope="col-2">Erholt</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in scope.districtsStatisticsArray
            .filter(filterFunction)
            .sort(compareFunction)
            .slice(0, 7 * scope.currentPage)" :key="index">
              <td>
                <RouterLink :to="'/district/' + item.ags">
                  {{ item.name }}
                </RouterLink>
                <br />
                ({{ item.state }})
              </td>
              <td>{{ item.population.toLocaleString() }}</td>
              <td>{{ item.cases.toLocaleString() }}</td>
              <td>{{ item.deaths.toLocaleString() }}</td>
              <td>{{ item.casesPerWeek.toLocaleString() }}</td>
              <td>{{ item.deathsPerWeek.toLocaleString() }}</td>
              <td>{{ item.recovered.toLocaleString() }}</td>
            </tr>
          </tbody>
        </table>
      </div>

    </ion-col>
  </ion-row>
  <ion-row>
    <ion-col lg="4" md="6">

      <ion-searchbar id="validationCustomUsername" placeholder="Welche Bundesland?"
        v-model="scope.filterName"></ion-searchbar>
    </ion-col>
    <ion-col lg="8" md="6">
      <ion-button v-if="scope.currentPage * 7 < scope.districtsStatisticsArray.length"
        @click="ionInfinite">Weiter</ion-button>

    </ion-col>
  </ion-row>
</ion-card></template>

<script setup lang="ts">
import { reactive, onMounted } from 'vue'
import axios from 'axios'
import { type MetaInformation, type DistrictsStatisticsSubType } from './types/dataTypes'
import {
  IonIcon, IonCol, IonRow, InfiniteScrollCustomEvent,
  IonCard, IonSearchbar, IonButton
} from '@ionic/vue';
import { closeOutline } from 'ionicons/icons';
const props = defineProps({
  stateAbbreviation: { type: String, required: false }
})

const scope: {
  filterName: string
  districtsStatisticsArray: Array<DistrictsStatisticsSubType>
  metaInformation: MetaInformation | null
  currentPage: number
  toastVisible: boolean
  isAlertVisible: boolean
} = reactive({
  filterName: '',
  districtsStatisticsArray: [],
  metaInformation: null,
  currentPage: 1,
  toastVisible: true,
  isAlertVisible: true
})

async function fetchDistrictsStatistics() {
  axios.get('https://api.corona-zahlen.org/districts').then((response) => {
    const responseData = response.data.data
    scope.metaInformation = response.data.meta

    const allValues: Array<DistrictsStatisticsSubType> = Object.values(responseData)
    if (props.stateAbbreviation !== undefined) {
      scope.districtsStatisticsArray = allValues.filter(
        (item: DistrictsStatisticsSubType) => item.stateAbbreviation === props.stateAbbreviation
      )
    } else {
      scope.districtsStatisticsArray = allValues
    }
  })
}

const ionInfinite = (ev: InfiniteScrollCustomEvent) => {

  scope.currentPage++;
};

// arg item: DistrictsStatisticsSubType - object element with statistic data
// return boolean - check substring filterName in item.name
const filterFunction = (item: DistrictsStatisticsSubType): boolean => {
  return item.name.toLowerCase().includes(scope.filterName.toLowerCase())
}

// function compare 2 arguments
// sort function returns a type number (-1, 0, 1)
const compareFunction = (a: DistrictsStatisticsSubType, b: DistrictsStatisticsSubType): number => {
  if (a.name > b.name) {
    return 1
  } else if (a.name < b.name) {
    return -1
  } else {
    return 0
  }
}

onMounted(() => {
  fetchDistrictsStatistics()
})
</script>

<template><ion-card no-body class="cpro_card-s">
  <ion-row>
    <ion-col lg="4">
      <div class="cpro_cell">
        <template v-if="scope.casesWeekCount !== null">
          <h1>{{ scope.casesWeekCount }}</h1>
          <span>Infektionen pro Woche

            <ion-icon :icon="informationCircleOutline" id="hover-trigger-1"></ion-icon>
            <ion-popover trigger="hover-trigger-1" trigger-action="hover">
              <ion-content class="ion-padding">Gesamtzahl aller gemeldeten pro letzte 7 Tage</ion-content>
            </ion-popover>


          </span>
        </template>

        <ion-spinner v-else></ion-spinner>
      </div>
    </ion-col>
    <ion-col lg="4">
      <div class="cpro_cell">
        <template v-if="scope.deathsWeekCount !== null">
          <h1>{{ scope.deathsWeekCount }}</h1>
          <span>Todesfälle pro Woche

            <ion-icon :icon="informationCircleOutline" id="hover-trigger-2"></ion-icon>
            <ion-popover trigger="hover-trigger-2" trigger-action="hover">
              <ion-content class="ion-padding">Gesamtzahl aller gemeldeten pro letzte 7 Tage</ion-content>
            </ion-popover>


          </span>
        </template>
        <ion-spinner v-else></ion-spinner>
      </div>
    </ion-col>
    <ion-col lg="4">
      <div class="cpro_cell">
        <template v-if="props.type === 'state'">
          <template v-if="scope.hospitalizationWeekCount !== null">
            <h1>{{ scope.hospitalizationWeekCount }}</h1>
            <span>Hospitalization pro Woche

              <ion-icon :icon="informationCircleOutline" id="hover-trigger-3"></ion-icon>
              <ion-popover trigger="hover-trigger-3" trigger-action="hover">
                <ion-content class="ion-padding">Gesamtzahl aller gemeldeten pro letzte 7 Tage</ion-content>
              </ion-popover>


            </span>
          </template>
          <ion-spinner v-else></ion-spinner>
        </template>
        <template v-else>
          <template v-if="scope.recoveredWeekCount !== null">
            <h1>{{ scope.recoveredWeekCount }}</h1>
            <span>Recovered pro Woche

              <ion-icon :icon="informationCircleOutline" id="hover-trigger-4"></ion-icon>
              <ion-popover trigger="hover-trigger-4" trigger-action="hover">
                <ion-content class="ion-padding">Gesamtzahl aller gemeldeten pro letzte 7 Tage</ion-content>
              </ion-popover>

            </span>
          </template>
          <ion-spinner v-else></ion-spinner>
        </template>
      </div>
    </ion-col>
  </ion-row>
</ion-card></template>

<script setup lang="ts">
import { reactive, onMounted } from 'vue'
import axios from 'axios'
import { useRoute } from 'vue-router'
//import type { BIconInfoCircle } from 'bootstrap-icons-vue'
import { type WeekCasesStatisticsType } from '../components/types/dataTypes'
import { type WeekDeathsStatisticsType } from '../components/types/dataTypes'
import { type WeekHospitalizationStatisticsType } from '../components/types/dataTypes'
import { type WeekRecoveredStatisticsType } from '../components/types/dataTypes'

import {
  IonCol, IonRow, IonCard, IonSpinner, IonPopover, IonContent, IonIcon
} from '@ionic/vue';
import { informationCircleOutline } from 'ionicons/icons';

const props = defineProps({
  type: { type: String, required: true }
})

const route = useRoute()

const scope: {
  casesWeekCount: null | number
  deathsWeekCount: null | number
  hospitalizationWeekCount: null | number
  recoveredWeekCount: null | number
} = reactive({
  casesWeekCount: null,
  deathsWeekCount: null,
  hospitalizationWeekCount: null,
  recoveredWeekCount: null
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

// function getAbbreviation(): string {
// return route?.params?.abbreviation.toString() ?? ''
// }

async function fetchCasesWeekStatistics() {
  axios
    .get('https://api.corona-zahlen.org/' + getURLAPI() + '/' + getURLIdent() + '/history/cases/7')
    .then((response) => {
      const weekCasesStatisticsArray: Array<WeekCasesStatisticsType> =
        response.data.data[getURLIdent()]['history']

      let initialValue = 0

      // map => convert array<WeekCasesStatisticsType> to array<number>
      // reduce => make sume
      // save all scope.casesWeekCount
      scope.casesWeekCount = weekCasesStatisticsArray
        .map((x) => x.cases)
        .reduce((accumulator, currentValue) => accumulator + currentValue, initialValue)
    })
}

async function fetchDeathsWeekStatistics() {
  axios
    .get('https://api.corona-zahlen.org/' + getURLAPI() + '/' + getURLIdent() + '/history/deaths/7')
    .then((response) => {
      const weekDeathsStatisticsArray: Array<WeekDeathsStatisticsType> =
        response.data.data[getURLIdent()]['history']

      let initialValue = 0
      scope.deathsWeekCount = weekDeathsStatisticsArray
        .map((x) => x.deaths)
        .reduce((accumulator, currentValue) => accumulator + currentValue, initialValue)
    })
}

async function fetchHospitalizationWeekStatistics() {
  axios
    .get('https://api.corona-zahlen.org/states/' + getURLIdent() + '/history/hospitalization/7')
    .then((response) => {
      const weekHospitalizationStatisticsArray: Array<WeekHospitalizationStatisticsType> =
        response.data.data[getURLIdent()]['history']

      let initialValue = 0
      scope.hospitalizationWeekCount = weekHospitalizationStatisticsArray
        .map((x) => x.cases7Days)
        .reduce((accumulator, currentValue) => accumulator + currentValue, initialValue)
    })
}

async function fetchRecoveredWeekStatistics() {
  axios
    .get('https://api.corona-zahlen.org/districts/' + getURLIdent() + '/history/recovered/7')
    .then((response) => {
      const weekRecoveredStatisticsArray: Array<WeekRecoveredStatisticsType> =
        response.data.data[getURLIdent()]['history']

      let initialValue = 0
      scope.recoveredWeekCount = weekRecoveredStatisticsArray
        .map((x) => x.recovered)
        .reduce((accumulator, currentValue) => accumulator + currentValue, initialValue)
    })
}

onMounted(() => {
  if (props.type === 'state') {
    fetchHospitalizationWeekStatistics()
  } else {
    fetchRecoveredWeekStatistics()
  }
  fetchCasesWeekStatistics()
  fetchDeathsWeekStatistics()
})
</script>

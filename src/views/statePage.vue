<template><ion-page>

  <ion-header :translucent="true">
    <ion-toolbar>
      <ion-buttons slot="start">
        <ion-menu-button color="primary"></ion-menu-button>
      </ion-buttons>
      <ion-title>{{ $route.params.id }}</ion-title>
    </ion-toolbar>
  </ion-header>


  <ion-content :fullscreen="true" class="ion-padding">
    <ion-grid>
      <ion-row>
        <ion-col>

          <state-dates :type="'state'" />

        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>

          <week-dates :type="'state'" />

        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>

          <age-statistics :type="'state'" />

        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>

          <gender-statistics :type="'state'" />

        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col>

          <districts-statistics v-if="getAbbreviation() != ''" :state-abbreviation="getAbbreviation()" />

        </ion-col>
      </ion-row>


    </ion-grid>


    <!--BContainer>
  <BRow style="margin-bottom: 20px">
    <BCol lg="6">
      <age-statistics :type="'state'" />
    </BCol>
    <BCol lg="6">
      <gender-statistics :type="'state'" />
    </BCol>
  </BRow>

  <BRow>
    <BCol>
      <districts-statistics v-if="getAbbreviation() != ''" :state-abbreviation="getAbbreviation()" />
    </BCol>
  </BRow>
</BContainer-->

  </ion-content>
</ion-page></template>

<script setup lang="ts">
import { useRoute } from 'vue-router'
import { IonCol, IonGrid, IonRow, IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar } from '@ionic/vue';

import ageStatistics from './../components/ageStatistics.vue'
import genderStatistics from './../components/genderStatistics.vue'
import stateDates from './../components/stateDates.vue'
import weekDates from './../components/weekDates.vue'
import districtsStatistics from '@/components/districtsStatistics.vue'

const route = useRoute()

function getAbbreviation(): string {
  return route?.params?.abbreviation?.toString() ?? ''
}
</script>

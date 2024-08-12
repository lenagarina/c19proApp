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
          <state-dates :type="'district'" />
          <week-dates :type="'district'" />
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>
          <age-statistics :type="'district'" />
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>
          <gender-statistics :type="'district'" />
        </ion-col>
      </ion-row>
    </ion-grid>
  </ion-content>
</ion-page></template>

<script setup lang="ts">
import ageStatistics from './../components/ageStatistics.vue'
import genderStatistics from './../components/genderStatistics.vue'
import stateDates from './../components/stateDates.vue'
import weekDates from './../components/weekDates.vue'

import { IonCol, IonGrid, IonRow, IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
</script>

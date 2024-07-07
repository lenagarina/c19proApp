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
          <span>
            Lena Garina <br />
            justline.lena@gmail.com <br />
            +49 176 6845 1881
          </span>
        </ion-col>
        <ion-col>
          <img src="../../src/assets/pics/personal-foto.jpg" />
        </ion-col>
      </ion-row>
    </ion-grid>

  </ion-content>
</ion-page></template>


<script setup lang="ts">
import { IonCol, IonGrid, IonRow, IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
</script>
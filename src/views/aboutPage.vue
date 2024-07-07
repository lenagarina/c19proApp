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
          <h2>C19Pro Projekt</h2>
          <h3 class="mt-2">Projekt basiert on Robert Koch-Institut API - von Marlon Lückert</h3>
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>

          <h3 class="mt-1">Technologie-Stack</h3>
          <ul>
            <li>Mobile Technologien: Application für Android und iOS</li>
            <li>Technologien: HTML, CSS, NodeJS</li>
            <li>Fontend Framework: Vue 3 with Composition API, Vite</li>
            <li>UI Framework: Ionic</li>
            <li>Style: SCSS, CSS</li>
            <li>Code Formatter: prettier, eslint</li>
            <li>Programmiersprachen: Typescript 5.3, Javascript</li>
            <li>VCS: Git</li>
            <li>Backend: Rest API Verbindung mit Robert Koch-Institut API (ajax, axios, json)</li>
          </ul>
        </ion-col>
        <ion-col>
          <img class="ion-margin-top" src="../../src/assets/pics/pro_logos.jpg" />
        </ion-col>
      </ion-row>

      <ion-row>
        <ion-col>

          <h3 class="mt-1">Übersicht</h3>
          <ul>
            <li>Anzahl der Infektionen, Todesfälle, Genesenen</li>
            <li>7-Tage-Inzidenz Wert</li>
            <li>Neuinfektionen, neue Todesfälle, neue Genesene (Differenz zum Vortag)</li>
            <li>R-Wert (Reproduktion)</li>
            <li>Impfquote</li>
            <li>Werte für jedes Bundesland und jeden Landkreis</li>
            <li>historische Daten für Deutschland, jedes Bundesland and jeden Landkreis</li>
            <li>Karten mit Bundesländern und Landkreisen</li>
            <li>Anzahl der wöchentlich durchgeführen PCR-Tests, Anzahl der positiven Tests sowie der Positiv Quote</li>
            <li>Fallzahlen und Todesfälle pro Altersgruppe</li>
            <li>Hospitalisierungsrate</li>

          </ul>
        </ion-col>
        <ion-col>
          &nbsp;
        </ion-col>
      </ion-row>
    </ion-grid>

  </ion-content>
</ion-page></template>


<script setup lang="ts">
import { IonCol, IonGrid, IonRow, IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
</script>
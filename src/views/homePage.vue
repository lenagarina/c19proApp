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
    <div class="cpro_carousel">
      <swiper :autoplay="true" :keyboard="true" :pagination="true" :scrollbar="true" :zoom="true">
        <swiper-slide>
          <div class="carousel-slide" style="background-image: url(/src/assets/pics/cpro_banner-1.jpg)">
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="carousel-slide" style="background-image: url(/src/assets/pics/cpro_banner-2.jpg)">
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="carousel-slide" style="background-image: url(/src/assets/pics/cpro_banner-3.jpg)">
          </div>
        </swiper-slide>
      </swiper>

      <div class="carousel-content">
        <div class="carousel-content-background"></div>
        <div class="carousel-content-texts">
          <h1>Corona Statistiken</h1>
          <h2>Aktuelle Corona Statistiken</h2>
          <span>Auf diese Seite befinden sich die Staat-, Bundesländer- und Kreis Statistiken auch
            nach Alter und Gender</span>
        </div>
      </div>
    </div>

    <ion-grid>
      <ion-row>
        <ion-col>
          <states-statistics />
        </ion-col>
      </ion-row>
      <ion-row>
        <ion-col>
          <districts-statistics />
        </ion-col>
      </ion-row>
    </ion-grid>
    <ion-grid class="cpro_footer-block">
      <ion-row class="cpro_footer-text">
        <ion-col>
          <p>
            <strong>Hintergrund zu COVID-19</strong> <br />
            Coronaviren wurden erstmals Mitte der 1960er Jahre identifiziert. Sie können sowohl
            Menschen als auch verschiedene Tiere infizieren. SARS-CoV-2 ist die offizielle
            Bezeichnung des neuartigen Coronavirus und COVID-19 die offizielle Bezeichnung der
            Erkrankung durch das neuartige Coronavirus.
          </p>
        </ion-col>
      </ion-row>

    </ion-grid>

    <ion-grid>
      <ion-row>
        <ion-col>
          <districts-map />
        </ion-col>
      </ion-row>
    </ion-grid>
    <footerBlock />
  </ion-content>


</ion-page></template>

<script setup lang="ts">
import statesStatistics from './../components/statesStatistics.vue'
import districtsStatistics from './../components/districtsStatistics.vue'
import districtsMap from './../components/districtsMap.vue'
import footerBlock from './../components/footerBlock.vue'

import { IonCol, IonGrid, IonRow, IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import '@ionic/vue/css/ionic-swiper.css';


</script>

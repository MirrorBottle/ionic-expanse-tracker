<template>
  <ion-header>
    <img :src="logo" alt="Ionic logo" style="height: 60px;">
  </ion-header>
</template>

<script>
import logo from "@/assets/Expanse-logo-white.png"
import { defineComponent } from 'vue';
import { IonHeader, IonToolbar, IonTitle } from '@ionic/vue';

export default defineComponent({
  name: 'Navbar',
  components: { IonHeader, IonToolbar, IonTitle  },
  data() {
    return { logo }
  }
});
</script>

<style>

</style>
<template>
  <ion-page>
    <Navbar />
    <ion-content :fullscreen="true" style="padding: 10px;">
      <ion-list class="ion-no-margin">
        <ion-item>
          <ion-label position="stacked">Nama Barang : </ion-label>
          <ion-input placeholder="placeholder" v-model="form.name"></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="stacked">Pengeluaran : </ion-label>
          <ion-input placeholder="placeholder" type="number" v-model="form.expanse"></ion-input>
        </ion-item>
      </ion-list>
      <ion-button expand="block" color="dark" @click="handleSubmit">Tambah</ion-button>
    </ion-content>
  </ion-page>
</template>

<script  >
import { defineComponent } from 'vue';
import { toastController, IonPage, IonList, IonItem, IonLabel, IonInput, IonContent, IonButton } from '@ionic/vue';
import Navbar from '@/components/Navbar.vue';
export default  defineComponent({
  name: 'Create',
  components: { Navbar, IonPage, IonList, IonItem, IonLabel, IonInput, IonContent, IonButton },
  data() {
    return {
      form: {
        expanse: 0,
        name: ''
      }
    }
  },
  methods: {
    async handleSubmit() {
      const expanses = JSON.parse(localStorage.getItem('EXPANSE_TRACKER_EXPANSES') || "[]");
      const data = {  
        ...this.form,
        date: new Date().toISOString().slice(0, 10)
      }
      const newExpanses = [...expanses, data];
      localStorage.setItem('EXPANSE_TRACKER_EXPANSES', JSON.stringify(newExpanses));
      this.form = { expanse: 0, name: '' }
      const toast = await toastController
        .create({
          message: 'Berhasil menambah pengeluaran.',
          duration: 1000,
          color: 'success'
        })
      return toast.present();
    }
  }
});
</script>

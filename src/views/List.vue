<template>
  <ion-page>
    <Navbar />
    <ion-content :fullscreen="true" style="padding: 10px;">
      <ion-list lines="full" class="ion-no-margin">
        <ion-item-divider>
          <ion-label>Rentang Waktu</ion-label>
        </ion-item-divider>
        <ion-item>
          <ion-label>Bulan</ion-label>
            <ion-select interface="popover" v-model="month" @ionChange="fetch">
              <ion-select-option v-for="month in months" :value="month.val">
                {{ month.name }}
              </ion-select-option>
            </ion-select>
        </ion-item>
        <ion-item>
          <ion-label>Tahun</ion-label>
            <ion-select interface="popover" v-model="year" @ionChange="fetch">
              <ion-select-option v-for="year in years" :value="year">
                {{ year }}
              </ion-select-option>
            </ion-select>
        </ion-item>
        <ion-item-divider>
          <ion-label>Pengeluaran</ion-label>
        </ion-item-divider>
        <ion-item>
          <div style="display: flex; align-items: center; justify-content: space-between; width: 100%;">
            <p><strong>Total</strong></p>
            <p><strong>{{ toRupiah(sum) }}</strong></p>
          </div>
        </ion-item>
        <ion-item v-for="expanse in expanses">
          <div style="display: flex; align-items: center; justify-content: space-between; width: 100%;">
            <p>{{ expanse.name }}</p>
            <p>{{ toRupiah(expanse.expanse) }}</p>
          </div>
        </ion-item>
        <ion-button expand="block" color="dark" @click="handleExport" style="margin-top: 20px;">Export</ion-button>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script  >
import { defineComponent } from 'vue';
import { IonPage, IonList, IonItem, IonLabel, IonButton, IonInput, IonSelect, IonSelectOption, IonItemDivider, IonContent } from '@ionic/vue';
import { Filesystem, Directory, Encoding } from '@capacitor/filesystem';
import Navbar from '@/components/Navbar.vue';

export default  defineComponent({
  name: 'List',
  components: { Navbar, IonPage, IonList, IonItem, IonLabel, IonButton, IonInput, IonSelect, IonSelectOption, IonItemDivider, IonContent },
  data() {
    return {
      expanses: [],
      month: new Date().toLocaleDateString("en-US", { month: "2-digit" }),
      year: new Date().getUTCFullYear(),
      sum: 0
    }
  },
  created() {
    this.fetch();
  },
  ionViewWillEnter() {
    this.fetch();
  },
  computed: {
    years() {
      const now = new Date().getUTCFullYear();    
      const years = Array(now - (now - 3)).fill('').map((v, idx) => now - idx);
      return years;
    },
    months() {
      return [
        { name: 'Januari', val: '01' },
        { name: 'Februari', val: '02' },
        { name: 'Maret', val: '03' },
        { name: 'April', val: '04' },
        { name: 'Mei', val: '05' },
        { name: 'Juni', val: '06' },
        { name: 'Juli', val: '07' },
        { name: 'Agustus', val: '08' },
        { name: 'September', val: '09' },
        { name: 'Oktober', val: '10' },
        { name: 'November', val: '11' },
        { name: 'Desember', val: '12' },
      ]
    }
  },
  methods: {
    fetch() {
      const expanses = JSON.parse(localStorage.getItem('EXPANSE_TRACKER_EXPANSES') || "[]");
      this.expanses = expanses.filter(expanse => expanse.date.includes(`${this.year}-${this.month}`));
      this.sum = this.expanses.map(expanse => Number(expanse.expanse)).reduce((a, b) => a + b, 0)
    },
    toRupiah(val) {
      return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' }).format(parseInt(val)).split(",00")[0]
    },
    async handleExport() {
      await Filesystem.writeFile({
        path: "expanses.txt",
        data: btoa(localStorage.getItem('EXPANSE_TRACKER_EXPANSES')), // your data to write (ex. base64)
        directory: Directory.External
      });
    }
  }
});
</script>

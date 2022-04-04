<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="tab2" href="/tabs/create">
          <ion-icon :icon="bagAddOutline" />
          <ion-label>Tambah</ion-label>
        </ion-tab-button>
        <ion-tab-button tab="tab1" href="/tabs/list">
          <ion-icon :icon="albumsOutline" />
          <ion-label>History</ion-label>
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script  >
import { defineComponent } from 'vue';
import { IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, IonPage, IonRouterOutlet } from '@ionic/vue';
import { bagAddOutline, albumsOutline } from 'ionicons/icons';

export default defineComponent({
  name: 'TabsPage',
  components: { IonLabel, IonTabs, IonTabBar, IonTabButton, IonIcon, IonPage, IonRouterOutlet },
  setup() {
    return {
      bagAddOutline, 
      albumsOutline,
    }
  }
});
</script>

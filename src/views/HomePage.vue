<template>
  <ion-content>
    <ion-list>
      <ion-item v-for="(item, index) in items" :key="index">
        <ion-avatar slot="start">
          <img :src="'https://picsum.photos/80/80?random=' + index" alt="avatar" />
        </ion-avatar>
        <ion-label>{{ item }}</ion-label>
      </ion-item>
    </ion-list>
    <ion-infinite-scroll @ionInfinite="ionInfinite">
      <ion-infinite-scroll-content></ion-infinite-scroll-content>
    </ion-infinite-scroll>
  </ion-content>
</template>

<script>
import {
  IonContent,
  IonInfiniteScroll,
  IonInfiniteScrollContent,
  IonList,
  IonItem,
  IonAvatar,
  IonImg,
  IonLabel,
} from '@ionic/vue';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  components: {
    IonContent,
    IonInfiniteScroll,
    IonInfiniteScrollContent,
    IonList,
    IonItem,
    IonAvatar,
    IonImg,
    IonLabel,
  },
  setup() {
    const items = ref([]);

    const generateItems = () => {
      const count = items.value.length + 1;
      for (let i = 0; i < 50; i++) {
        items.value.push(`Item ${count + i}`);
      }
    };

    const ionInfinite = (ev) => {
      generateItems();
      setTimeout(() => ev.target.complete(), 500);
    };

    generateItems();

    return { ionInfinite, items };
  },
});
</script>

<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Galeria de Fotos</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Galeria</ion-title>
        </ion-toolbar>
      </ion-header>
      
      <ion-grid>
        <ion-row>
          <ion-col size="6" size-md="4" size-xl="3" v-for="(photo, index) in photos" :key="index">
            <ion-img :src="photo.url" @click="showPhoto(photo)"></ion-img>
          </ion-col>
        </ion-row>
      </ion-grid>

      <!-- FAB for adding photos -->
      <ion-fab vertical="bottom" horizontal="center" slot="fixed">
        <ion-fab-button @click="triggerFileInput">
          <ion-icon :icon="camera"></ion-icon>
        </ion-fab-button>
      </ion-fab>

      <!-- Hidden file input -->
      <input type="file" accept="image/*" ref="fileInput" @change="onFileSelected" style="display: none;" />

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonRow, IonCol, IonImg, IonFab, IonFabButton, IonIcon } from '@ionic/vue';
import { camera } from 'ionicons/icons';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Tab2',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonGrid, IonRow, IonCol, IonImg, IonFab, IonFabButton, IonIcon },
  setup() {
    return {
      camera
    }
  },
  data() {
    return {
      photos: [] as { id: number; url: string }[]
    }
  },
  created() {
    this.loadPhotos();
  },
  methods: {
    loadPhotos() {
      const savedPhotos = localStorage.getItem('userPhotos');
      if (savedPhotos) {
        this.photos = JSON.parse(savedPhotos);
      }
    },
    triggerFileInput() {
      (this.$refs.fileInput as HTMLInputElement).click();
    },
    onFileSelected(event: any) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          const result = e.target?.result as string;
          const newPhoto = {
            id: Date.now(),
            url: result
          };
          this.photos.unshift(newPhoto);
          this.savePhotos();
        };
        reader.readAsDataURL(file);
      }
      // Reset input so validation triggers can fire again for same file if needed
      event.target.value = '';
    },
    savePhotos() {
      localStorage.setItem('userPhotos', JSON.stringify(this.photos));
    },
    showPhoto(photo: any) {
       // Optional: Could open a modal here, for now just log
       console.log('Selected photo', photo);
    }
  }
});
</script>
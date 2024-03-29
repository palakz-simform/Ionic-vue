<template>
  <base-layout page-title="Add a Memory" page-default-back-link="/memories">
    <form class="ion-padding" @submit.prevent="addMemory">
      <ion-list>
        <ion-input v-model="memoryData.title" label="Title" label-placement="floating" type="text" />
        <!-- <ion-input
        v-model="memoryData.imageUrl"
        label="Image URL"
        label-placement="floating"
        type="url"/> -->
        <ion-textarea v-model="memoryData.description" label="Description" label-placement="floating" />
        <ion-grid>
          <ion-row>
            <ion-col class="image-content">
              Image
            </ion-col>
            <ion-col  class="camera-content">
                <ion-icon :icon="addd" style="color: blueviolet;" size="large"  @click="clickImage"></ion-icon>
            </ion-col>
          </ion-row>
          <ion-row>
            <ion-col v-if="imageUrl">
                <ion-img :src="imageUrl" class="ion-float-center"></ion-img>
              
            </ion-col>
          </ion-row>
        </ion-grid>
        <ion-button expand="block" class="ion-margin-top " @click="addMemory">
          Submit
        </ion-button>
      </ion-list>
    </form>
  </base-layout>
</template>
<script setup>
import { IonList, IonInput, IonTextarea, IonButton, IonImg, IonThumbnail, IonIcon, IonGrid, IonRow, IonCol, } from '@ionic/vue'
import { useMemoryStore } from "../stores/memory";
import { storeToRefs } from "pinia";
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { camera } from 'ionicons/icons'
import { Camera, CameraResultType } from '@capacitor/camera';
import { Filesystem, Directory, Encoding } from '@capacitor/filesystem';

const addd = ref(camera)
const router = useRouter()
const memoryData = ref({})
const memoryStore = useMemoryStore();
const { memoryList } = storeToRefs(memoryStore);
const addMemory = () => {
  const newMemory = {
    id: new Date().toISOString(),
    title: memoryData.value.title,
    image: imageUrl.value,
    description: memoryData.value.description
  }
  memoryList.value.unshift(newMemory)
  router.replace('/memories')
}
const render = ref(false)
const imageUrl = ref('')

const clickImage = async () => {
  await Camera.getPhoto({
    quality: 90,
    allowEditing: false,
    resultType: CameraResultType.Uri
  }).then((image) => {
    render.value = true;
    imageUrl.value = String(image.webPath);
  });
};

</script>
<style scoped>
.camera-content{
  display: flex;
  justify-content: end;
}
.image-content{
  display: flex;
  align-items: center;
  padding-left: 0px;
}
ion-img{
  height: 250px !important;
}
</style>
<template>
  <base-layout page-title="Add a Memory" page-default-back-link="/memories">
    <form class="ion-padding" @submit.prevent="addMemory">
      <ion-list>
        <ion-input v-model="memoryData.title" label="Title" label-placement="floating" type="text" />
        <ion-textarea v-model="memoryData.description" label="Description" label-placement="floating" />
         <ion-row>
        <ion-col size="10.5">
    <ion-input v-model="memoryData.loaction" label="Location" label-placement="floating" type="text" />
  </ion-col>
  <ion-col size="1.5" class="locationIcon">
    <ion-icon :icon="locationIcon" style="color: blueviolet;" size="large"  @click="createMap" ></ion-icon>
  </ion-col>
        </ion-row>
        <ion-grid>
          <ion-row>
            <ion-col class="image-content">
              Image
            </ion-col>
            <ion-col  class="camera-content">
                <ion-icon :icon="cameraIcon" style="color: blueviolet;" size="large"  @click="clickImage"></ion-icon>
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
import { camera,location } from 'ionicons/icons'
import { Camera, CameraResultType } from '@capacitor/camera';
import { Geolocation } from '@capacitor/geolocation';
import { GoogleMap } from '@capacitor/google-maps';

const cameraIcon = ref(camera)
const locationIcon = ref(location)
const router = useRouter()
const memoryData = ref({})
const memoryStore = useMemoryStore();
const { memoryList } = storeToRefs(memoryStore);
const addMemory = () => {
  console.log(memoryData.value.loaction);
  const newMemory = {
    id: new Date().toISOString(),
    title: memoryData.value.title,
    image: imageUrl.value,
    location: memoryData.value.loaction,
    description: memoryData.value.description
  }
  memoryList.value.unshift(newMemory)
  console.log(memoryList.value);
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
const getCurrentPosition = async () => {
  const coordinates = await Geolocation.getCurrentPosition();

  console.log('Current position:', coordinates.coords);
  fetchData(coordinates.coords.latitude,coordinates.coords.longitude)
};
async function fetchData(latitude,longitude) {
  const url = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=76795dbae9ef2f0dc863fa136fa8762a`;
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();
    memoryData.value.loaction = data.name
  } catch (error) {
    console.error('There was a problem with the fetch operation:', error);
  }
}
getCurrentPosition()

const mapOpen = ref(false)
const myDiv = ref(null)
const createMap = async () => {
 router.push('/memories/map');
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
.locationIcon{
  margin-top: 10px;
}
.map-container {
  width: 100%;
  height: 100vh; /* 100% viewport height */
}
</style>
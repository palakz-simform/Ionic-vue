<template>
    <base-layout page-title="Add a Memory" page-default-back-link="/memories">
    <form class="ion-padding" @submit.prevent="addMemory">
        <ion-list>
             <ion-input
             v-model="memoryData.title"
        label="Title"
        label-placement="floating"
        type="text"/>
        <ion-input
        v-model="memoryData.imageUrl"
        label="Image URL"
        label-placement="floating"
        type="url"/>
         <ion-textarea
        v-model="memoryData.description"
        label="Description"
        label-placement="floating"/>
        <ion-button expand="block"  class="ion-margin-top " @click="addMemory">
          Submit
        </ion-button>
        </ion-list>
    </form>
    </base-layout>
</template>
<script setup>
import {IonList,IonInput,IonTextarea,IonButton} from '@ionic/vue'
import { useMemoryStore } from "../stores/memory";
import { storeToRefs } from "pinia";
import {ref} from 'vue'
import {useRouter} from 'vue-router'
const router = useRouter()
const memoryData = ref({})
const memoryStore = useMemoryStore();
const { memoryList } = storeToRefs(memoryStore);
const addMemory = () =>{
   const newMemory = {
    id: new Date().toISOString(),
    title:memoryData.value.title,
    image:memoryData.value.imageUrl,
    description:memoryData.value.description
   }
   memoryList.value.unshift(newMemory)
   router.replace('/memories')

}

</script>
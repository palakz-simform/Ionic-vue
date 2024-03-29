<template>
    <base-layout page-title="All Memories">
    <template v-slot:actions-end>
    <ion-buttons router-link="/memories/add">
        <ion-icon slot="icon-only" :icon="addd"></ion-icon>
    </ion-buttons>
    </template>
          <MemoriesList :memories="memories"/>
</base-layout>

</template>
<script setup>
import {IonButtons,IonIcon} from '@ionic/vue'
import {add} from 'ionicons/icons'
import {ref} from 'vue'
import MemoriesList from '../components/memories/MemoriesList.vue'
import { useMemoryStore } from '../stores/memory'
const {memories} = useMemoryStore()
console.log(memories);
const addd = ref(add)
</script>
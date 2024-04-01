<template>
    <base-layout page-title="Map">
        <!-- <MemoriesList :memories="memories" /> -->
        <capacitor-google-map ref="myDiv"></capacitor-google-map>

    </base-layout>

</template>
<script setup>
import { IonButtons, IonIcon } from '@ionic/vue'
import { GoogleMap } from '@capacitor/google-maps';
import { add } from 'ionicons/icons'
import { ref, onMounted } from 'vue'
import MemoriesList from '../components/memories/MemoriesList.vue'
import { useMemoryStore } from '../stores/memory'
const { memories } = useMemoryStore()
const addd = ref(add)
const myDiv = ref(null)
onMounted(() => {
    console.log(myDiv.value)
    createMap()
})
const createMap = async () => {
    const mapRef = myDiv.value;

    const newMap = await GoogleMap.create({
        id: 'my-map', // Unique identifier for this map instance
        element: mapRef, // reference to the capacitor-google-map element
        apiKey: 'AIzaSyB7INKMFjCoMAP0DnEOXcy0nxEzOIKCtkU', // Your Google Maps API Key
        config: {
            center: {
                // The initial position to be rendered by the map
                lat: 33.6,
                lng: -117.9,
            },
            zoom: 8, // The initial zoom level to be rendered by the map
        },
    });
    newMap.addMarker({
        position: {
            lat: 23.014509,
            lng: 72.591759,
        },
        title: 'Marker Title',
        snippet: 'Marker Description',
    });
    // newMap.addListener('click', (event) => {
    //     // Get the clicked location
    //     const clickedLocation = event.latLng;

    //     // Do something with the clicked location, e.g., add a marker
    //     newMap.addMarker({
    //         position: clickedLocation,
    //         title: 'Selected Location',
    //     });
    // });
};
</script>
<style>
capacitor-google-map {
    display: inline-block;
    width: 100%;
    height:100%;
}
</style>
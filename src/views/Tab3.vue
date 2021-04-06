<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search</ion-title>
      </ion-toolbar>
      <ion-toolbar>
        <ion-searchbar debounce="500" :onIonChange="(e) => fetchSearchResults(e.detail.value)"></ion-searchbar>
      </ion-toolbar>
    </ion-header>
       <ion-content v-if="state.loading">
        <div class="loading-center">
          <ion-spinner color="primmary"></ion-spinner>
        </div>  
        </ion-content>
    <ion-content :fullscreen="true" v-else>
      <div class="center" v-if="state.searchResults && state.searchResults.length === 0">
        <ion-label>No results </ion-label>
      </div>
      <drink-card v-for="drink in state.searchResults" :key="drink.idDrink" :drink="drink"></drink-card>

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent,IonSearchbar } from '@ionic/vue';
import {reactive} from "vue";
import axios from "axios";
import DrinkCard from "@/components/DrinkCard.vue";
import IDrinkDetails from "../interfaces/IDrinksDetails"

export default  {
  name: 'Tab3',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonSearchbar,DrinkCard },
  setup(){
    const state = reactive({
      searchResults: [] as IDrinkDetails [],
      loading: false
    });

    const fetchSearchResults = async (searchTerm: string) =>{
      state.loading = true; 

      if (searchTerm){
        state.searchResults = [];
        const res = await axios.get(`https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${searchTerm}`);
   
      if (res.data){
        state.searchResults = res.data?.drinks
        } 
      }

      state.loading = false; 
    };

    return{
      state,
      fetchSearchResults
    }
  }
}
</script>

<style scoped>
.loading-center{
  display:flex;
  align-items: center;
  justify-content: center;
}
</style>
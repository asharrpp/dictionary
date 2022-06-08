<template>
  <form @submit.prevent="getMeaning()">
    <input placeholder="Search Word" type="text" id="word" v-model="word" />
    <button type="submit">Fetch Meaning</button>
  </form>
  <p v-if="!ValidQuery">Error : the word you are searching for does not exist</p>
  <div v-for="Meaning in definitionsArray" :key="Meaning">
    <div>{{Meaning}}</div>
  </div>
</template>

<script>
import axios from "axios";
import { ref } from 'vue';
export default {
  name: "App",
  setup() {
    let ValidQuery = ref(true);
    const definitionsArray = ref([]);
    const word = ref("");
    
    async function getMeaning() {
      definitionsArray.value.splice(0,definitionsArray.value.length)
       try {
      const url =
        "https://api.dictionaryapi.dev/api/v2/entries/en/" + word.value;

      const { data } = await axios.get(url);
      console.log(data);
      data.map(({ meanings }) => {
        meanings.map(({ definitions }) => {
          definitions.forEach(({ definition }) => {
            if (typeof definition === "string")
              definitionsArray.value.push(definition);
          });
        });
      });
       } 
       catch (error) {
       ValidQuery.value = false;
       }
    }
    return { word, getMeaning, definitionsArray,ValidQuery };
  }
}
</script>

<style></style>

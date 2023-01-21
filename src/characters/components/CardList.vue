<script setup lang="ts">
import { ref } from 'vue'
import breakingBadApi from '@/api/breakingBadApi';
import type{ ResponseData, Character } from '@/characters/interfaces/character'
import { useCharacters } from '@/characters/composables/useCharacters';
import { useQuery } from '@tanstack/vue-query';


//! Composable Functions
// const { characters, isLoading, hasError, errorMessage} = useCharacters();
const getCharactersSlow = async():Promise<Character[]> =>{

    const {results} = await (await breakingBadApi.get<ResponseData>('/character')).data;
    
    console.log('results::: ', results);
   
    return results
}

const { isLoading, isError, results:characters, error} = useQuery(
    ['characters'],
    getCharactersSlow,
)
</script>
<template>
   <h1 v-if="isLoading">Loading...</h1>
    <h1 v-if="isError">{{ error }}</h1>
    <ul>
        <li 
            v-for="{id, name} of characters"
            :key="id"
        >
        {{ name }}
        </li>
    </ul>
  
</template>


<style scoped>

</style>
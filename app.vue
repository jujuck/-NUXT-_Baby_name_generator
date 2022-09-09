<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the 'Find Names' button below</p>
    <div class="options-container">
      <Option 
        v-for="option in optionsArray"
        :key="option.category"
        :option="option"
        :options="options"
      />
      <button class="primary" @click="computedSelectedNames">Click to validate</button>
      <div class="cards-container">
        <CardName v-for="(name, index) in selectedNames" 
          :key="name" 
          :name="name" 
          @remove="() => removeName(index)"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {Gender, Popularity, Length, names} from "@/data"

  interface OptionsState {
    gender: Gender;
    popularity: Popularity;
    length: Length
  }

  const options = reactive<OptionsState>({
    gender: Gender.GIRL,
    length: Length.ALL,
    popularity: Popularity.TRENDY
  })

  const selectedNames = ref<string[]>([]);

  const computedSelectedNames = () => {
    selectedNames.value = names
      .filter((name) => name.gender === options.gender 
        && (name.length === options.length || options.length === Length.ALL) 
        && name.popularity === options.popularity)
      .map(name => name.name)
  }

  const removeName = (index: number) => {
    const filteredNames = [...selectedNames.value];
    filteredNames.splice(index, 1);
    selectedNames.value = filteredNames;

  }

  const optionsArray = [
    {
      title: "1) Choose a gender",
      buttons: [Gender.GIRL, Gender.BOY, Gender.UNISEX],
      category: "gender"
    },
    {
      title: "1) Choose a name's popularity",
      buttons: [Popularity.UNIQUE, Popularity.TRENDY],
      category: "popularity"
    },
    {
      title: "1) Choose a length",
      buttons: [Length.ALL, Length.SHORT, Length.LONG],
      category: "length"
    }
  ]
</script>


<style scoped>
.container {
  font-family: Arial, Arial, Helvetica, sans-serif;
  color: rgb(27,60,138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

.container h1 {
  font-size: 3rem;
}

.options-container {
  background-color:rgb(255,238,236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;;
}


.primary {
  background-color: rgb(249,87,89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer
}

.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}
</style>
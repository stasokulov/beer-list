<template>
  <div>
    <ul class="list">
      <DrinkItem v-for="drink in drinksList"
                 :key="drink.id"
                 :drink="drink"
                 @delete-drink="deleteDrink($event)"
                 @edit-drink="editDrink($event)"
      />
    </ul>
    <button @click="addDrinks" v-if="showButton">
      <span v-if="!loading">Show next</span>
      <span v-if="loading">Loading...</span>
    </button>
  </div>
</template>

<script>
import { ref } from 'vue'
import DrinkItem from './components/DrinkItem.vue'

export default {
  name: 'App',
  components: {
    DrinkItem
  },
  created () {
    this.getDrinksList()
  },
  setup () {
    const drinksList = ref([])
    const linkToDrinks = ref('https://api.punkapi.com/v2/beers')
    const page = ref(1)
    const showButton = ref(false)
    const loading = ref(false)
    const getDrinksList = async function () {
      const url = `${linkToDrinks.value}?page=${page.value}`
      try {
        const newDrinksList = await fetch(url)
          .then(response => response.json())
          .then(json => {
            return json
          })
        if (newDrinksList.length === 0) {
          showButton.value = false
        } else {
          loading.value = false
          drinksList.value = [...drinksList.value, ...newDrinksList]
          showButton.value = true
        }
      } catch (error) {
        console.error(error)
      }
    }
    const addDrinks = function () {
      loading.value = true
      page.value++
      getDrinksList()
    }
    const deleteDrink = function (id) {
      drinksList.value.forEach((drink, index) => {
        if (drink.id === id) {
          drinksList.value.splice(index, 1)
        }
      })
    }
    const editDrink = function (array) {
      const id = array[0]
      const name = array[1]
      const description = array[2]
      drinksList.value.forEach((drink) => {
        if (drink.id === id) {
          drink.name = name
          drink.description = description
        }
      })
    }
    return {
      drinksList,
      linkToDrinks,
      page,
      showButton,
      loading,
      getDrinksList,
      addDrinks,
      deleteDrink,
      editDrink
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  ul {
    list-style-type: none;
    padding: 0;
    text-align: left;
  }
}
</style>

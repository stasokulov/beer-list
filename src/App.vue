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
import DrinkItem from './components/DrinkItem.vue'

export default {
  name: 'App',
  components: {
    DrinkItem
  },
  created () {
    this.getDrinksList()
  },
  data () {
    return {
      drinksList: [],
      linkToDrinks: 'https://api.punkapi.com/v2/beers',
      page: 1,
      showButton: false,
      loading: false
    }
  },
  methods: {
    getDrinksList: async function () {
      const url = `${this.linkToDrinks}?page=${this.page}`
      try {
        const drinksList = await fetch(url)
          .then(response => response.json())
          .then(json => {
            return json
          })
        if (drinksList.length === 0) {
          this.showButton = false
        } else {
          this.loading = false
          this.drinksList = [...this.drinksList, ...drinksList]
          this.showButton = true
        }
      } catch (error) {
        console.error(error)
      }
    },
    addDrinks: function () {
      this.loading = true
      this.page++
      this.getDrinksList()
    },
    deleteDrink: function (id) {
      this.drinksList.forEach((drink, index) => {
        if (drink.id === id) {
          this.drinksList.splice(index, 1)
        }
      })
    },
    editDrink: function (array) {
      const id = array[0]
      const name = array[1]
      const description = array[2]
      this.drinksList.forEach((drink) => {
        if (drink.id === id) {
          drink.name = name
          drink.description = description
        }
      })
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

<template>
  <div id="app">
    <div class="showcase">
      <Item v-for="item in mockedData.products" v-bind:key="item.id" v-bind:item="item"/>
    </div>
    <div class="favorites">
      <h2>My favorites</h2>
      <div v-for="item in favorites" v-bind:key="item.id" class="favorite-item">
        <span class="item__name">{{item.name}}</span>
        <span class="item__price">{{item.price}}₽</span>
      </div>
      <div class="favorites__total">
        Total: {{amount}} items / {{total}}₽
      </div>
    </div>
  </div>
</template>

<script>
import Item from './components/Item.vue'
import mockedData from './fixtures/products.json'
import {eventBus} from './main'

export default {
  name: 'App',
  data () {
    return {
      mockedData: null,
      favorites: [],
      total: 0,
      amount: 0
    }
  },
  components: {
    Item
  },
  created() {
    this.mockedData = mockedData;

    eventBus.$on('addToFavs', data => {
      this.total = 0;
      let pickedItem = data.item;
      const item = this.favorites.find(item => item.id === pickedItem.id);

      if (item) {
        for (let i = 0; i < this.favorites.length; i++) {
          if (this.favorites[i].name == item.name) {
            this.favorites.splice(i, 1);
          }
        }
      }
      else {
        this.favorites.push(pickedItem);
      }

      this.favorites.forEach(element => this.total = this.total + element.price);
      this.amount = this.favorites.length;

    })

  }
}
</script>

<style>
#app {
  display: flex;
  flex-direction: row;
}

.showcase {
  width: 1000px;
  display: grid;
  grid-template-columns: repeat(3, 316px);
  grid-template-rows: repeat(5, 402px);
  grid-column-gap: 16px;
  grid-row-gap: 64px; 
}

.favorites {
  display: flex;
  flex-direction: column;
  font-family: 'Raleway', sans-serif;
}

.favorite-item {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.item__name {
  font-size: 20px;
  margin-right: 48px;
}

.item__price {
  font-size: 24px;
}

.favorites__total {
  margin-top: 8px;
  padding-top: 4px;
  border-top: 1px solid #000;
  text-align: right;
  font-size: 20px;
}
</style>

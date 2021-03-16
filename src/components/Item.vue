<template>
  <div class="item" v-bind:class="{runningOut: runningOut}">
    <div class="item__body">
      <button class="faveThisBtn" @click="addToFavs(item)"></button>
    </div>
    <div class="item__info">
      <h1>{{item.name}}</h1>
      <div class="item__params">
        <span class="item__price">{{item.price}}₽</span>
        <span class="item__qty">На складе: {{item.qty}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import {eventBus} from '../main'

export default {
  name: 'Item',
  props: {
    item: Object,
  },
  data () {
    return {
      runningOut: false
    }
  },
  methods: {
    addToFavs: function(item) {
      if (item.qty === 0) {
        alert('Товар закончился!');
      }
      else {
        eventBus.$emit('addToFavs', {
          item: item
        });
      }
    }
  },
  created() {
    if (this.item.qty <= 3) {
      this.runningOut = true;
    }
  }
}
</script>

<style scoped>
.item {
  width: 304px;
  height: 400px;
  margin: 12px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.runningOut .item__body {
  opacity: 0.8;
  background-color: rgba(255,0,0,0.2);
}

.item__body {
  width: 304px;
  height: 338px;
  background: url('../assets/unnamed.png');
  background-color: #fff;
  background-position: top 40px right 20px;
  background-repeat: no-repeat;
  box-shadow: 2px 6px 10px rgba(1,1,0,0.1);
  display: flex;
  flex-direction: row;
  justify-content: end;
  align-items: flex-end;
  transition-delay: 0.4s;
  transition: 0.4s;
  opacity: 0.9;
}

.item__body:hover {
  transition-delay: 0.4s;
  transition: 0.4s;
  box-shadow: 2px 7px 10px rgba(1,1,0,0.2);
  background-position: top 40px right 0px;
  opacity: 1;
}

.faveThisBtn {
  width: 32px;
  height: 32px;
  padding: 0; margin: 0; border: 0;
  background: url('../assets/heart.png');
  background-size: 100% 100%;
  margin-right: 4px;
  margin-bottom: 4px;
  cursor: pointer;
}

.faveThisBtn:hover {
  background: url('../assets/heart-hover.png');
  background-size: 100% 100%;
}

.item__info {
  margin-top: 10px;
  width: 304px;
  height: 50px;
  font-family: 'Raleway', sans-serif;
}

.item__params {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.item__price, .item__qty {
  letter-spacing: .8px;
  color: #666;
}

h1 {
  letter-spacing: .5px;
  color: #2d2d2d;
  font-size: 14px;
}
</style>

<template>
  <div class="container">
    <add-item
    :currentItems="items"
    @addNewItem="updateItems"/>
    <ul class="product-list">
      <h2>You have: {{ wallet | currency }}</h2>
      <li class="product-list__item"
        v-for="item in items"
        :key="item.name"
        v-bind:class="{expensive: wallet < item.price, added: item.added}">
        <div @click="updateBasket($event, item)">
          <span class="item-name">{{ item.name }}</span>
          <span class="item-price">{{ item.price | currency}}</span>
        </div>
        <span class="remove" @click="removeItem(item.name)">x</span>
      </li>
      <transition name="slide">
        <p class="error" v-if="toExpensive">You don't have enough money to buy <span class="item">{{ currentItemName }}</span></p>
      </transition>
      <h2>Your basket: {{ sum | currency }}</h2>
    </ul>
  </div>
</template>

<script>
import AddItem from '@/components/AddItem.vue'

export default {
  name: 'ShopList',
  components: {
    'add-item': AddItem
  },
  data() {
    return {
      wallet: 100,
      basket: [],
      sum: 0,
      toExpensive: false,
      currentItemName: '',
      items: [
        {name: 'Apples', price: 5.0},
        {name: 'Shoes', price: 250.5},
        {name: 'Pants', price: 48.0},
        {name: 'Tomatos', price: 3.48},
        {name: 'Milk', price: 2.75},
        {name: 'Flowers', price: 50.75}
      ]
    }
  },
  methods: {
    updateBasket: function(event, item) {
      const basketIndex = this.basket.indexOf(item)
      const listIndex = this.items.indexOf(item)

      this.toExpensive = false

      if(item.price <= this.wallet && basketIndex < 0) {
        this.basket.push(item)
        this.wallet -= item.price
        this.sum += item.price
        this.items[listIndex].added = true
      } else if(basketIndex !== -1) {
        this.basket.splice(basketIndex, 1)
        this.wallet += item.price
        this.sum -= item.price
        this.items[listIndex].added = false
      } else {
        this.toExpensive = true
        this.currentItemName = item.name
      }
    },
    updateItems: function(newItem) {
      this.items.push(newItem)
    },
    removeItem: function(itemName) {
      const item = this.items.find(el => el.name === itemName)
      const itemIndex = this.items.indexOf(item)
      this.items.splice(itemIndex, 1)
    }
  },
  filters: {
    currency: function(price) {
      return '$' + price.toFixed(2)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  *,
  *::before,
  *::after {
    box-sizing: border-box;
  }
  :root {
    --flame: #e4572e;
    --cyan: #30bced;
    --lapis-lazuli: #33658a;
    --android-green: #99c24d;
    --dark-jungle-green: #041b15;
    --white: #fff;
    --border-radius: 3px;
  }

  .body {
    color: var(--dark-jungle-green)
  }

  .container {
    width: 100%;
    max-width: 1200px;
    margin: 5rem auto;
  }

  .product-list {
    width: 500px;
    list-style: none;
    margin: 0 auto;
    padding: 1rem 2rem;
    background-color: var(--cyan);
    border-radius: var(--border-radius);
    &__item {
      position: relative;
      margin: .75rem 0;
      font-size: 1.25rem;
      background-color: var(--lapis-lazuli);
      color: var(--white);
      border-radius: var(--border-radius);
      cursor: pointer;
      transition: background-color linear .2s, color linear .4s;
      div {
        display: flex;
        justify-content: space-between;
        padding: 1rem;
      }
      &.expensive {
        background-color: var(--flame);
        cursor: not-allowed;
      }
      &.added {
        background-color: var(--android-green);
        color: var(--dark-jungle-green);
        cursor: pointer;
      }
      .item-price {
        font-weight: bold;
      }
      .remove {
        position: absolute;
        display: inline-block;
        width: 0;
        left: 100%;
        top: 16px;
        overflow: hidden;
        background-color: var(--flame);
        color: var(--white);
        border-top-right-radius: var(--border-radius);
        border-bottom-right-radius: var(--border-radius);
        transition: width linear .2s;
      }
      &:not(.added) {
        &:hover {
          .remove {
            width: 30px;
          }
        }
      }
    }
  }
  .error {
    background-color: var(--flame);
    color: var(--white);
    margin: 1rem auto;
    padding: .5rem .75rem;
    border-radius: var(--border-radius);
    font-weight: bold;
    max-width: 90%;
    .item {
      display: inline-block;
      padding: .33rem;
      background-color: var(--white);
      color: var(--flame);
      border-radius: var(--border-radius);
    }
  }

  .slide {
    &-enter,
    &-leave-to {
      opacity: 0;
      margin-top: -50px;
    }
    &-enter-active,
    &-leave-active {
      transition: all linear .25s;
    }
  }
</style>

<template>
  <div class="container">
  <ul class="product-list">
    <h2>You have: {{ wallet | currency }}</h2>
    <li class="product-list__item"
      v-for="item in items"
      :key="item.name"
      @click="updateBasket($event, item)"
      v-bind:class="{expensive: wallet < item.price}">
      <span class="item-name">{{ item.name }}</span>
      <span class="item-price">{{ item.price | currency }}</span>
    </li>
    <p class="error" v-if="toExpensive">You don't have enough money to buy <span class="item">{{ currentItemName }}</span></p>
    <h2>Your basket: {{ sum | currency }}</h2>
  </ul>
  </div>
</template>

<script>
export default {
  name: 'ShopList',
  data() {
    return {
      wallet: 100,
      basket: [
      ],
      sum: 0,
      toExpensive: 0,
      currentItemName: '',
      items: [
        {name: 'Apples', price: 5.0},
        {name: 'Shoes', price: 250.5},
        {name: 'Pants', price: 48.0},
        {name: 'Tomatos', price: 3.48},
        {name: 'Milk', price: 2.75}
      ]
    }
  },
  methods: {
    updateBasket: function(event, item) {
      const el = event.target
      const itemIndex = this.basket.indexOf(item)
      if(item.price <= this.wallet && itemIndex < 0) {
        this.toExpensive = false
        this.basket.push(item)
        this.wallet -= item.price
        this.sum += item.price
        el.classList.remove('expensive')
        el.classList.add('added')
      } else if(itemIndex > -1) {
        this.toExpensive = false
        this.basket.splice(itemIndex, 1)
        el.classList.remove('added')
        this.wallet += item.price
        this.sum -= item.price
      } else {
        this.toExpensive = true
        this.currentItemName = item.name
      }
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
    width: 400px;
    list-style: none;
    margin: 0 auto;
    padding: 1rem 2rem;
    background-color: var(--cyan);
    border-radius: var(--border-radius);
    &__item {
      display: flex;
      justify-content: space-between;
      padding: 1rem;
      margin: .75rem;
      font-size: 1.25rem;
      background-color: var(--lapis-lazuli);
      color: var(--white);
      border-radius: var(--border-radius);
      cursor: pointer;
      &.added {
        background-color: var(--android-green);
        color: var(--dark-jungle-green);
      }
      &.expensive {
        background-color: var(--flame);
        cursor: not-allowed;
      }
      .item-price {
        font-weight: bold;
      }
    }

    .error {
      color: var(--flame);
      font-weight: bold;
      font-size: 1rem;
      .item {
        display: inline-block;
        padding: .33rem;
        background-color: var(--flame);
        color: var(--white);
        border-radius: var(--border-radius);
      }
    }
  }
</style>

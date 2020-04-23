<template>
  <div class="new-item-box">
    <input type="text" v-model="item.name" placeholder="Name">
    <input type="number" v-model="item.price" placeholder="price">
    <button type="button" class="btn" @click="addNewItem">Add</button>
    <transition name="slide">
      <p class="error" v-if="error">Wrong item! At least 3 signs and price greater than 0.</p>
    </transition>
    <transition name="slide">
      <p class="error" v-if="duplicate">This item already exists on the list.</p>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'AddItem',
  props: ['currentItems'],
  data() {
    return {
      item: {
        name: '',
        price: ''
      },
      error: false,
      duplicate: false
    }
  },
  methods: {
    addNewItem: function() {
      this.error = false // In case that previously user tried to add wrong item.
      this.duplicate = false
      this.item.name = this.item.name.charAt(0).toUpperCase() + this.item.name.slice(1)

      const itemExists = this.currentItems.find(el => el.name === this.item.name)
      if(itemExists) {
        this.duplicate = true
        return ''
      }
      if(this.item.name.length > 2 && this.item.price > 0) {
        this.item.price = Number(this.item.price) // Received String but need Number to display price in proper format.
        this.$emit('addNewItem', this.item)
      } else {
        this.error = true
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .new-item-box {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 500px;
    margin: 2rem auto;
    padding: 1rem 2rem;
    background-color: var(--lapis-lazuli);
    border-radius: var(--border-radius);
    input {
      margin-bottom: 1rem;
      padding: .33rem;
      border: none;
      border: 2px solid var(--cyan);
      border-radius: var(--border-radius);
      &[type=text] {
        width: 60%;
      }
      &[type=number] {
        width: 15%;
      }
    }
    .btn {
      width: 100%;
      padding: .5rem;
      border: none;
      border-radius: var(--border-radius);
      background-color: var(--android-green);
      color: var(--white);
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
    }
  }
</style>

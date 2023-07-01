<template>
  <div :key="item.name" class="menu-item">
    <img
      class="menu-item__image"
      :src="item.image.source"
      :alt="item.image.alt"
    />
    <div>
      <h3>{{ item.name }}</h3>
      <p>Prix : {{ generatedPrice }}</p>
      <p v-if="item.inStock">En stock</p>
      <p v-else>En rupture de stock</p>
      <div>
        <label for="add-item-quantity">Quantité : {{ item.quantity }}</label>
        <input
          @input="$emit('update:quantity', $event)"
          id="add-item-quantity"
          type="number"
        />
        <button @click="addToShoppingCart?.(item.quantity)">
          Ajouter au panier
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "MenuItem",
  props: {
    item: { type: Object, required: true },
    addToShoppingCart: Function,
  },
  data() {
    return {
      onSale: false,
    };
  },
  computed: {
    generatedPrice(): number {
      return this.onSale ? (this.item.price * 0.9).toFixed(2) : this.item.price;
    },
  },
  beforeMount() {
    const today = new Date().getDate();

    if (today % 2 === 0) {
      this.onSale = true;
    }
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.description {
  max-width: 960px;
  font-size: 1.2rem;
  margin: 0 auto;
}

.footer {
  text-align: center;
  font-style: italic;
}

.menu {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.menu-item {
  display: flex;
  width: 500px;
  justify-content: space-between;
  margin-bottom: 30px;
}

.menu-item__image {
  max-width: 300px;
}

.shopping-cart {
  position: absolute;
  right: 30px;
  top: 0;
}
</style>

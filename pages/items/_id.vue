<template>
  <!-- <div>
    <pre>{{ currentItem }}</pre>
  </div> -->
  <main class="container">
    <section
      class="image"
      :style="`background: url(/${currentItem.img}) no-repeat center center`"
    ></section>

    <section class="details">
      <h1>{{ currentItem.item }}</h1>
      <h1>Price: ${{ currentItem.price }}</h1>

      <div class="quantity">
        <input type="number" min="1" v-model="cartNumber" />
        <button @click="addToCart" class="primary">
          Add to Cart -{{ cartValue }}
        </button>
      </div>
      <fieldset v-if="currentItem.options">
        <legend>Options</legend>
        <div v-for="opt in currentItem.options" :key="opt">
          <input
            :id="opt"
            type="radio"
            name="Options"
            :value="opt"
            v-model="itemOptions"
          />
          <label :for="opt">{{ opt }}</label>
        </div>
      </fieldset>
      <fieldset v-if="currentItem.addOns">
        <legend>Add Ons</legend>
        <div v-for="add in currentItem.addOns" :key="add">
          <input
            :id="add"
            type="checkbox"
            name="addOns"
            :value="add"
            v-model="itemAddOns"
          />
          <label :for="add">{{ add }}</label>
        </div>
      </fieldset>

      <AppToast v-if="cartSubmitted"
        >Order Submitted <br />
        Check out more
        <nuxt-link to="/restaurants">restaurants</nuxt-link>!</AppToast
      >
    </section>

    <section class="options">
      <h3>Description</h3>
      <p>{{ currentItem.description }}</p>
    </section>
    <!-- <pre>{{ currentItem }}</pre> -->
  </main>
</template>

<script>
import { mapState } from "vuex";
import AppToast from "@/components/AppToast.vue";

export default {
  components: {
    AppToast,
  },
  data() {
    return {
      id: this.$route.params.id,
      cartNumber: 1,
      itemOptions: "",
      itemAddOns: [],
      itemSizeAndCost: [],
      cartSubmitted: false,
    };
  },
  computed: {
    ...mapState(["fooddata"]),
    cartValue() {
      return this.cartNumber * this.currentItem.price;
    },
    currentItem() {
      let result;
      for (let i = 0; i < this.fooddata.length; i++) {
        for (let j = 0; j < this.fooddata[i].menu.length; j++) {
          if (this.fooddata[i].menu[j].id === this.id) {
            result = this.fooddata[i].menu[j];
            break;
          }
        }
      }
      return result;
    },
  },
  methods: {
    addToCart() {
      let formOutput = {
        item: this.currentItem.item,
        count: this.cartNumber,
        options: this.itemOptions,
        addOns: this.itemAddOns,
        combinedPrice: this.cartValue,
      };
      this.cartSubmitted = true;
      this.$store.commit("addToCart", formOutput);
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  width: 1000px;
  margin: 100px auto;
  display: grid;
  grid-template-columns: 400px 1fr;
  grid-template-rows: 400px 1fr;
  grid-column-gap: 60px;
  grid-row-gap: 60px;
  line-height: 2;
}

.image {
  grid-area: 1 / 1 / 2 / 2;
  background-size: cover;
}
.details {
  grid-area: 1 / 2 / 2 / 3;
  position: relative;
}
.options {
  grid-area: 2 / 1 / 3 / 2;
}
</style>
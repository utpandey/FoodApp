<template>
  <!-- <div>
    <pre>{{ currentItem }}</pre>
  </div> -->
  <main class="container parent">
    <section
      class="image div1"
      :style="`background: url(/${currentItem.img}) no-repeat center center`"
    ></section>
    <section class="details div2">
      <h1>{{ currentItem.item }}</h1>
      <h1>Price: ${{ currentItem.price }}</h1>
      <div class="quantity">
        <input type="number" min="1" v-model="cartNumber" />
        <button class="primary">Add to Cart -{{ cartValue }}</button>
      </div>
    </section>

    <fieldset class="div3">
      <legend>Options</legend>
      <div v-for="opt in currentItem.options" :key="opt">
        <input :id="opt" type="checkbox" name="Options" :value="opt" />
        <label :for="opt">{{ opt }}</label
        ><br />
      </div>
    </fieldset>
    <fieldset class="div4">
      <legend>Add Ons</legend>
      <div v-for="add in currentItem.addOns" :key="add">
        <input :id="add" type="checkbox" name="addOns" :value="add" />
        <label :for="add">{{ add }}</label
        ><br />
      </div>
    </fieldset>
    <section class="options div5">
      <h3>Description</h3>
      <p>{{ currentItem.description }}</p>
    </section>
    <pre>{{ currentItem }}</pre>
  </main>
  <!-- <div class="parent">
    <div class="div1"></div>
    <div class="div2"></div>
    <div class="div3"></div>
    <div class="div4"></div>
    <div class="div5"></div>
  </div> -->
</template>

<script>
import { mapState } from "vuex";

export default {
  data() {
    return {
      id: this.$route.params.id,
      cartNumber: 1,
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
};
</script>

<style lang="scss" scoped>
.parent {
  display: grid;
  grid-template-columns: 2fr repeat(2, 1fr) repeat(2, 0fr);
  grid-template-rows: repeat(3, 1fr);
  grid-column-gap: 35px;
  grid-row-gap: 20px;
}

.div1 {
  grid-area: 1 / 1 / 3 / 2;
}
.div2 {
  grid-area: 1 / 2 / 2 / 4;
}
.div3 {
  grid-area: 2 / 2 / 3 / 4;
}
.div4 {
  grid-area: 3 / 2 / 4 / 4;
}
.div5 {
  grid-area: 3 / 1 / 4 / 2;
}
</style>
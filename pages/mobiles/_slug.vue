<template>
  <div>
    <h2 v-bind:style="{ color: mobile.colour}"> {{ mobile.title }}</h2>
    <nuxt-content :document="mobile" />
     <div v-for="field in mobile.displayFields" :key="field">
       <h3> {{product[field]}}</h3>
     </div>
  
  </div>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ $content, params, error }) {
    let mobile;
    let data;
    let product;
    try {
      mobile = await $content("mobile", params.slug).fetch();
      data = await axios.get("https://www.samsung.com/uk/api/v4/configurator/syndicated-product?sku="+mobile.title);
      product = data.data.products[0];
    } catch (e) {
      error({ message: e });
    }

    return {
      mobile,
      product
    };
  },
};
</script>
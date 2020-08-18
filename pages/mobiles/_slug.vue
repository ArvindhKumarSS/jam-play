<template>
  <div>
    <h2>{{ mobile.title }}</h2>
    <nuxt-content :document="mobile" />
    <h3> {{field1}}</h3> </br>
    <h3> {{field2}}</h3>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData({ $content, params, error }) {
    let mobile;
    let data;
    let field1, field2;
    try {
      mobile = await $content("mobile", params.slug).fetch();
      data = await axios.get("https://www.samsung.com/de/api/v4/configurator/syndicated-product?sku=EF-BT870PAEGEU");
      field1 = data.data.products[0][mobile.field1]
      field2 = data.data.products[0][mobile.field2]
    } catch (e) {
      error({ message: e });
    }

    return {
      mobile,
      field1,
      field2
    };
  },
};
</script>
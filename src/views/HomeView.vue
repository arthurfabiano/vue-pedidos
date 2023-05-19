<template>
  <Banner/>
  <div class="main-container">
      <h1>Monte seu Pedido</h1>
      <Form/>
  </div>
</template>

<script>

import {defineComponent} from "vue";
import Banner from "@/components/Banner.vue";
import Form from "@/components/Form.vue";
export default defineComponent({
    name: 'Home',
    components: {Banner, Form}
})
</script>

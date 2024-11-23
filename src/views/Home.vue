<template>
  <Banner />
  <div class="main-container">
    <h1>Monte seu burger:</h1>
    <BurgerForm />
  </div>
  <ul>
    <li v-for="item in data" :key="item.id">{{ item.name }}</li>
  </ul>
</template>

<script>
import Banner from "../components/Banner.vue"
import BurgerForm from "../components/BurgerForm.vue"

export default {
  name: "Home",
  components: {
    Banner,
    BurgerForm
  },
}
</script>
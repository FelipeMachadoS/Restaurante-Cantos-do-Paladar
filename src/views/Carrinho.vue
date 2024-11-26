<template>
  <div class="main-container">
    <h1>Confirme seu Pedido:</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '../components/Dashboard.vue'

export default {
  name: "Carrinho",
  components: {
    Dashboard
  }
}
</script>
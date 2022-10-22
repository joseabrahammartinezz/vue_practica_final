<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Apicación de Inventarios"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/Bienvenida.vue'

export default {
  name: 'InicioView',
  components: {
    HelloWorld
  }
}
</script>

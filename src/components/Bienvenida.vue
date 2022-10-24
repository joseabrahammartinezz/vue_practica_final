<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      <h2>Práctica Final Módulo VI - Vue 3</h2>
    </p>
    <h1>José Abraham Martínez Soliz</h1>
    
  </div>
</template>

<script>
export default {
  name: 'Bienvenida',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

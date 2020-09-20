<template>

  <div id="app">

    <h1> Calculadora - Básica </h1>
    
    <Main/>

  </div>

</template>

<script>
import Main from './main/Main'

export default {
  components: {Main}
}
</script>

<style>
body{
  margin:0
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  text-align: center;
  height: 100vh;
  color: #000000;
  background: linear-gradient(180deg, rgba(255,255,255,1) 0%, rgba(180,180,180,1) 100%);
}
</style>

<template>

  <div class="display">
    <p>{{values}}</p>
  </div>

</template>

<script>
export default {
  name: "Display",
  props:['values']
}
</script>

<style>

.display{
  grid-column: span 4;
  background-color: rgba(0, 0, 0, 0.788);
  color: rgba(255, 255, 255, 0.836);
  
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 20px 5px 5px 5px;
}

.display p {
    font-size: 1.7em;
    overflow: hidden;
    max-width: 300px;
    margin: 0;
}
</style>
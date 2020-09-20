<template>

  <button
    class="botao"
    :class="{operation, double, triple, formula}"
    @click="$emit('onClick',label)">
        {{label}}
    </button>

</template>

<script>
export default {
  name: "Button",

  props: {
    label: {},
    operation: { type: Boolean },
    double: { type: Boolean },
    triple: { type: Boolean },
    formula: { type: Boolean },
  },
};
</script>

<style>
:root {
  --bg-button: #ffffff;
  --bg-button-hover: rgba(255, 255, 255, 0.219);
  --border-button: solid 1px #888;
}

.botao {
  font-size: 1.4em;
  font-weight: bold;
  background-color: var(--bg-button);
  border: none;
  border-right: var(--border-button);
  border-top: var(--border-button);
  outline: none;
}
.botao:hover {
  background-color: var(--bg-button-hover);
}

.botao.double {
  grid-column: span 2;
}

.botao.triple {
  grid-column: span 3;
}

.botao.operation {
  background-color: rgb(17, 192, 104);
  color: whitesmoke;
}

.botao.formula {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode";
  font-size: 1.3em;
}

.botao.operation:active {
  background-color: rgb(12, 88, 50);
  color: whitesmoke;
}

.botao:active {
  background-color: #888;
}
</style>
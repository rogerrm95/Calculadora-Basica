<template>
  <div class="calculadora">
    <Display :values="displayValues" />
    <Botao label="%" formula @onClick="setOperation" />
    <Botao label="AC" double @onClick="clearMemory" />
    <Botao label="←" @onClick="shift" />
    <Botao label="√²" formula @onClick="squareRoot" />
    <Botao label="1/x" formula @onClick="oneByN" />
    <Botao label="x²" formula @onClick="pow2" />
    <Botao label="/" operation @onClick="setOperation" />
    <Botao label="7" @onClick="add" />
    <Botao label="8" @onClick="add" />
    <Botao label="9" @onClick="add" />
    <Botao label="*" operation @onClick="setOperation" />
    <Botao label="4" @onClick="add" />
    <Botao label="5" @onClick="add" />
    <Botao label="6" @onClick="add" />
    <Botao label="-" operation @onClick="setOperation" />
    <Botao label="1" @onClick="add" />
    <Botao label="2" @onClick="add" />
    <Botao label="3" @onClick="add" />
    <Botao label="+" operation @onClick="setOperation" />
    <Botao label="+/-" @onClick="plusMinus" />
    <Botao label="0" @onClick="add" />
    <Botao label="." operation @onClick="add" />
    <Botao label="=" operation @onClick="setOperation" />
  </div>
</template>

<script>
import Botao from "../components/button";
import Display from "../components/display";

export default {
  name: "Main",
  data: function () {
    return {
      displayValues: "0",
      operation: null,
      clearDisplay: false,
      values: [0, 0],
      current: 0,
    };
  },
  components: { Display, Botao },
  methods: {
    // Limpa a memoria(display) //
    clearMemory() {
      // retorna o estado inicial do objeto
      Object.assign(this.$data, this.$options.data());
    },

    //Inverte de negativo <-> positivo //
    plusMinus() {
      const current = this.displayValues;
      this.values[0] = -current;
      this.displayValues = this.values[0];
    },

    // Raiz quadrada //
    squareRoot() {
      const current = this.displayValues;
      this.values[0] = Math.sqrt(current);
      this.displayValues = isNaN(this.values[0])
        ? "Entrada Inválida"
        : this.values[0];
    },

    // Potencia com expoente 2 //
    pow2() {
      const current = this.displayValues;
      this.values[0] = Math.pow(current, 2);
      this.displayValues = this.values[0];
    },
    // Divisão na base 1 //
    oneByN() {
      const current = this.displayValues;
      this.values[0] = 1 / current;
      this.displayValues = this.values[0];
    },

    // Apaga ultimo número digitado //
    shift() {
      const current = this.current;
      let value = this.values[current].toString();

      if (value.length <= 1) {
        this.values[current] = 0;
        this.displayValues = 0;
      } else {
        let newValue = value.substring(0, value.length - 1);
        this.values[current] = newValue;
        this.displayValues = this.values[current];
      }
    },

    // Realiza a operação entre dois números e um operador //
    setOperation(operation) {
      if (this.current === 0) {
        this.operation = operation;
        this.current = 1;
        this.clearDisplay = true;
      } else {
        const equals = operation === "=";
        const percentual = operation === "%";

        if (percentual) {
          this.values[1] = this.values[1] / 100;
          this.displayValues = this.values[1];
        } else {
          try {
            const currentOperation = this.operation;

            // Verificando se os numeros são inteiros ou não-inteiros e transformando-os em vetores ([Inteiro,Decimal])
            let value1 = Number.isInteger(this.values[0])
              ? this.values[0].toFixed(1)
              : this.values[0].toString();
            value1 = value1.split(".");
            let value2 = Number.isInteger(this.values[1])
              ? this.values[1].toFixed(1)
              : this.values[1].toString();
            value2 = value2.split(".");

            let result = this.precisionDecimal(
              value1,
              value2,
              currentOperation
            );

            this.values[0] = parseFloat(result);
          } catch (e) {
            this.$emit("onError", e);
          }

          this.values[1] = 0;
          this.displayValues = this.values[0];
          this.operation = equals ? null : operation;
          this.current = equals ? 0 : 1;
          this.clearDisplay = !equals;
        }
      }
    },

    // Realiza a entrada dos dígitos //
    add(n) {
      if (n === "." && this.displayValues.includes(".")) {
        return;
      }
      const clearDisplay = this.displayValues === "0" || this.clearDisplay;
      const currentDisplay = clearDisplay ? "" : this.displayValues;
      const displayValues = currentDisplay + n;

      this.displayValues = displayValues;
      this.clearDisplay = false;

      if (n !== ".") {
        const i = this.current;
        const newValue = parseFloat(displayValues);
        this.values[i] = newValue;
      }
    },

    // Controla a precisão dos numeros decimais //
    // Recebe dois vetores contendo a parte inteira e decimal//
    // Verifica qual dos dois vetores possui a casa decimal maior//
    // Atribui ao toFixed() o numero com a casa decimal maior, com isso obtendo uma precisão exata
    precisionDecimal(array1, array2, operation) {
      let max = array1[1].toString().length;

      if (array1[1].toString().length !== array2[1].toString().length) {
        if (array1[1].toString().length > array2[1].toString().length) {
          max = array1[1].toString().length;
        } else {
          max = array2[1].toString().length;
        }
      }
      let result;

      switch (operation) {
        case "+":
          result = (
            parseFloat(`${array1[0]}.${array1[1]}`) +
            parseFloat(`${array2[0]}.${array2[1]}`)
          ).toFixed(max);
          break;
        case "-":
          result = (
            parseFloat(`${array1[0]}.${array1[1]}`) -
            parseFloat(`${array2[0]}.${array2[1]}`)
          ).toFixed(max);
          break;
        case "*":
          result = (
            parseFloat(`${array1[0]}.${array1[1]}`) *
            parseFloat(`${array2[0]}.${array2[1]}`)
          ).toFixed(max + 1);
          break;
        case "/":
          result =
            parseFloat(`${array1[0]}.${array1[1]}`) /
            parseFloat(`${array2[0]}.${array2[1]}`);
          break;
        default:
          result = 0;
      }

      return result;
    },
  },
};
</script>

<style>
.calculadora {
  height: 500px;
  width: 300px;
  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 1fr 13% 13% 13% 13% 13% 13%;
  border: solid 2px black;
  box-shadow: black 1px 1px 7px;
}
</style>  
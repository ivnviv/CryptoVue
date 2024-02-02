<template>
  <input min=0 type="number" @input="changeAmount($event.target.value)" placeholder="Введите сумму"> <br>
  <button @click="convert">Конвертировать</button>

</template>

<script>
export default {
  props: {
    changeAmount: {
      type: Function,
      required: true
    },
    convert: {
      type: Function,
      required: true
    },
  }
}
</script>

<style scoped>
input {
  outline: none;
  border-radius: 3px;
  border: 0;
  padding: 10px 15px;
  background: #fafafa;
  color: #333;
  width: 500px;
  top: 50px;
  font-size: 1.2em;
}
button {
  position: relative;
  top: -20px;
  margin-top: 40px;
  padding: 15px 20px;
  color: #FFF;
  text-transform: uppercase;
  cursor: pointer;
  background: #1A032D;
  border: 0;
  border-radius: 3px;
}
</style>
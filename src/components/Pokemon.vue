<template>
  <div class="cortina">
    <img v-if="img" v-bind:src="img" alt="No se puede visualizar"/>
  </div>
  <p>{{ nombre }}</p>
</template>
  
  <script>
export default {
  props: {
    mostrar: false
  },
  data() {
    return {
      img: null,
      nombre: "xxxxxxxxxx",
    };
  },
};
</script>
  
<style>
.cortina {
  background-color: black;
  height: 300px;
  width: 300px;
}
img {
  height: 300px;
  width: 300px;
}
</style>
<template>
  <li v-bind:class="{on}">

  </li>
</template>

<script>
export default {
  name: 'Cell',
  props: {
    on: Boolean
  }
}
</script>

<style >
.cell {
  width: 10px;
  height: 10px;
  background: transparent;  
}
.cell.on {
  background: #42b983;  
}

</style>

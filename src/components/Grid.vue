<template>
<div>
  <div>
    <input type="number" v-model.number="rows" @change="somefunction">
    <input type="number" v-model.number="columns" @change="somefunction">
  </div>
  <div class="grid">
    <ul v-for="(row, index) in table" :key="index">
      <Cell
        v-bind:on="Boolean(item2)"
        class="cell"
        v-for="(item2, index2) in table[index]"
        :key="`${index} ${index2}`"
        >{{ item2 }}</Cell
      >
    </ul>
  </div>
</div>
</template>

<script>
import Cell from "./Cell";

export default {
  name: "Grid",
  components: {
    Cell,
  },
  data: function() {
    return {
      rows: 30,
      columns: 30,
      intervalId: null,
      table: this.generateGrid(this.rows || 30, this.columns || 30)
    };
  },
  computed: {

  },
  methods: {

    log(message) {
      console.log(message);
    },
    somefunction() {
      this.table = this.generateGrid(this.rows,this.columns)
    },
    generateGrid(
      rows,
      columns,
      mapper = () => Boolean(Math.floor(Math.random() * 2))
    ) {
      return new Array(rows)
        .fill()
        .map(() => new Array(columns).fill().map(mapper));
    },
    
    countActiveNeighbours(rowIdx, colIdx, indexedGrid) {
      let count = 0;
      for (let x = -1; x <= 1; x++) {
        for (let y = -1; y <= 1; y++) {
          if (!x && !y) continue;
          indexedGrid[rowIdx + x]?.[colIdx + y] && count++;
        }
      }
      return count;
    },
    animate(cb) {
      return (this.intervalId = setInterval(cb, 100));
    },
    stopAnimate() {
      clearInterval(this.intervalId);
      return (this.intervalId = null);
    },
    proceed() {
      this.table = this.table.map((row, rowIdx, arr) =>
        row.map((value, colIdx) => {
          const activeNeighbours = this.countActiveNeighbours(rowIdx,colIdx,arr);
          return value
            ? activeNeighbours === 2 || activeNeighbours === 3 // initially alive cell
            : activeNeighbours === 3; // initially dead cell
        })
      );
    },
  },
  created: function() {
    this.animate(this.proceed)
    this.countActiveNeighbours(10, 10, this.table);

  },
};
</script>

<style>
.grid {
  font-size: 0;
}
ul {
  list-style: none;
  display: block;
}
li {
  display: inline-block;
}
</style>

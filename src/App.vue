<template>
  <div id="app">
    <h1>Items Management Page</h1>
    <form @submit.prevent="addToListIn">
      <input
        type="text"
        v-model="listMember"
        placeholder="Enter Item Name and Click Add"
      />
      <button type="submit" @click="addToListIn">Add</button>
    </form>

    <div class="dataIn">
      <ul>
        <li
          v-for="(list, index) in listIn"
          :key="index"
          @click="moveRight(index)"
        >
          {{ list }}
        </li>
      </ul>
      <button>➡️</button>
    </div>

    <div class="dataOut">
      <ul>
        <li
          v-for="(list, index) in listOut"
          :key="index"
          @click="moveLeft(index)"
        >
          {{ list }}
        </li>
      </ul>
      <button type="submit">⬅️</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      listMember: "",
      selectedMember: 0,
      listIn: [],
      listOut: [],
    };
  },
  methods: {
    addToListIn() {
      this.listIn.push(this.listMember);
      this.listMember = "";
    },
    moveRight(index) {
      let outBoundValue = this.listIn.splice(index, 1);
      this.listOut.push(outBoundValue[0]);
    },
    moveLeft(index) {
      let inBoundValue = this.listOut.splice(index, 1);
      this.listIn.push(inBoundValue[0]);
    },
  },
  components: {},
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  text-align: left;
}

ul {
  text-decoration: none;
  list-style-type: none;
}
</style>

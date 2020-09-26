<template>
  <div id="app">
    <p>Logo</p>
    <p><span>Home</span> <span>About Us</span></p>
    <h1>Items Management Page</h1>

    <div class="dataIn">
      <form @submit.prevent="addToListIn">
        <input
          type="text"
          v-model="listMember"
          placeholder="Enter Item Name and Click Add"
        />
        <button type="submit" @click="addToListIn">Add</button>
      </form>
      <ul>
        <li
          v-for="(list, index) in listIn"
          :key="index"
          @click="moveRight(index)"
        >
          {{ list }}
        </li>
      </ul>
    </div>
    <div>
      <button>➡️</button>
      <button>⬅️</button>
    </div>
    <div class="dataOut">
      <h1>Selected Items</h1>
      <ul>
        <li
          v-for="(list, index) in listOut"
          :key="index"
          @click="moveLeft(index)"
        >
          {{ list }}
        </li>
      </ul>
    </div>

    <hr />
    <p><span>Home</span> <span>About Us</span></p>
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
      dataUrl: "https://items-manager-api.herokuapp.com/",
    };
  },
  methods: {
    async fetchList() {
      let itemsList = await fetch(`${this.dataUrl}`);
      let response = await itemsList.json();
      console.log(response);
      this.listIn = response[0].in;
      this.listOut = response[0].out;
    },
    async postList(data) {
      let response = await fetch(`${this.dataUrl}`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: data,
      });

      return await response.json();
    },
    addToListIn() {
      this.listIn.push(this.listMember);
      this.listMember = "";
    },
    persistItems() {
      let data = JSON.stringify({ in: this.listIn, out: this.listOut });
      let response = this.postList(data);
      console.log(response);
    },
    moveRight(index) {
      let outBoundValue = this.listIn.splice(index, 1);
      this.listOut.push(outBoundValue[0]);
      this.persistItems();
    },
    moveLeft(index) {
      let inBoundValue = this.listOut.splice(index, 1);
      this.listIn.push(inBoundValue[0]);
      this.persistItems();
    },
  },
  components: {},
  mounted: function() {
    this.fetchList();
  },
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
  margin: auto;
  width: 900px;
}

ul {
  text-decoration: none;
  list-style-type: none;
}

.dataIn,
.dataOut {
  width: 50%;
}
</style>

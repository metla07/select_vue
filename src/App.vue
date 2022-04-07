<template>
  <div id="app">
    <div class="container">
      <select v-model="selectedUser">
        <option v-for="(user, index) in cartsInfo.users" :key="index" :value="user.id">{{user.name}}</option>
      </select>
      <button @click="reset()"
              class="reset_btn"
              >RESET
      </button>
      <select v-model="selectedProduct">
        <option v-for="(product, index) in cartsInfo.products" :key="index" :value="product.id">{{product.name}}</option>
      </select>
    </div>
    <ul v-for="(user, index) in computedItems" :key="index">
      <li class="name_li">{{user.name}}</li>
      <li v-html="showProducts(user)"></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      // MODELS
      selectedUser: '',
      selectedProduct: '',
      // DATA
      cartsInfo: {
        users: [
          {
            id: 1,
            name: "Alex",
          },
          {
            id: 2,
            name: "Sam",
          },
          {
            id: 3,
            name: "Elon",
          },
          {
            id: 4,
            name: "Miki",
          },
          {
            id: 5,
            name: "Anna",
          },
          {
            id: 6,
            name: "Tom",
          },
          {
            id: 7,
            name: "Bob",
          },
          {
            id: 8,
            name: "Elisa",
          },
          {
            id: 9,
            name: "Roxy",
          },
          {
            id: 10,
            name: "Bill",
          },
        ],
        products: [
          {
            id: 1,
            name: "Phone",
          },
          {
            id: 2,
            name: "TV",
          },
          {
            id: 3,
            name: "Lego",
          },
          {
            id: 4,
            name: "Hammer",
          },
          {
            id: 5,
            name: "Umbrella",
          },
          {
            id: 6,
            name: "Backpack",
          },
          {
            id: 7,
            name: "Glasses",
          },
          {
            id: 8,
            name: "Mixer",
          },
          {
            id: 9,
            name: "Brush",
          },
          {
            id: 10,
            name: "Battery",
          }
        ],
        carts: [
          {
            userId: 1,
            productsIds: [1, 2, 5],
          },
          {
            userId: 2,
            productsIds: [1, 3, 2, 4],
          },
          {
            userId: 3,
            productsIds: [1, 10],
          },
          {
            userId: 4,
            productsIds: [7, 6],
          },
          {
            userId: 5,
            productsIds: [1, 3, 8],
          },
          {
            userId: 6,
            productsIds: [1, 3, 9],
          },
          {
            userId: 7,
            productsIds: [4, 3, 5],
          },
          {
            userId: 8,
            productsIds: [6 , 1, 8],
          },
          {
            userId: 9,
            productsIds: [2, 4, 10],
          },
          {
            userId: 10,
            productsIds: [1, 4],
          },
        ],
      }
    };
  },
  computed: {
    computedItems: function () {
      let filterByUser = this.selectedUser;
      let filterByProduct = this.selectedProduct;
      return this.cartsInfo.users.filter(_user => {
        let filtered = true;
          if (filterByUser)
            filtered = _user.id === filterByUser
          if (filtered && filterByProduct) {
            let usersFilteredByProduct = this.cartsInfo.carts.filter(cart => cart.productsIds.includes(filterByProduct));
            filtered = usersFilteredByProduct.filter(cart => cart.userId === _user.id).length;
          }
        return filtered;
      });
    }
  },
  methods: {
    showProducts(user) {
      let data = this.cartsInfo;
      let productsIds = data.carts.find(cart => cart.userId === user.id).productsIds; // [1,2,3]
      let products = data.products;
      let names = []; // [ "Phone", "Lego", "TV" ]
        productsIds.forEach(id => {names.push(products.find(product => product.id === id).name)});
      return names.join().replace(/,/g, '<br><hr>');
    },
    reset() {
      let userReset = (this.selectedUser = '');
      let productReset = (this.selectedProduct = '');
      return {userReset,productReset};
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*margin: 10px auto;*/
  width: 100%;
  min-height: 800px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  padding-top: 40px;
}
.container {
  display: flex;
  justify-content: center;
  width: 100%;
  min-height: 100px;
  position: absolute;
  top: 20px;
}
.reset_btn {
  margin-top: 12px;
  width: 100px;
  height: 50px;
  outline: none;
  border: 2px solid red;
  border-radius: 10px;
  background-color: black;
  color: white;
  transition: 4s;
}
.reset_btn:hover {
  background-color: red;
  transition: 1.3s;
}
select {
  margin: 10px;
  border: 4px solid darkgrey;
  border-radius: 10px;
  outline: none;
  text-align: center;
  line-height: 55px;
  height: 55px;
  width: 200px;
  position: relative;
  top: 0;
  font-size: 25px;
  transition: 0.3s;
}
select:hover {
  border: 4px solid darkgreen;
  color: darkgreen;
  transition: 0.3s;
}
option {
  font-size: 20px;
  color: blue;
}
li {
  display: inline-block;
  font-size: 20px;
  margin-bottom: 10px;
}
.name_li {
  margin: 5px 0 15px 0;
  font-size: 35px;
  color: darkblue;
}
ul{
  display: flex;
  flex-direction: column;
  border: 4px solid black;
  border-radius: 10px;
  width: 380px;
  height: 250px;
  margin: 15px 0 0 0;
  padding: 10px;
  box-shadow: 1px 1px 15px grey;
  transition: 0.5s;
}
ul:hover {
  border: 4px solid green;
  box-shadow: 1px 1px 15px green;
  transition: 0.3s;
}
</style>


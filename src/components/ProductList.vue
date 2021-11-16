<template>
  <div class="product-list">
    <div class="flex top-information">
      <h1>Товары</h1>
      <div class="flex search-wrapper">
        <my-input @add="addNewProduct" @check="validateInput" />
        <template v-if="tooltipProducts.length">
          <ul class="search-tooltips">
            <li
              v-for="(product, index) in tooltipProducts"
              :key="index"
              @click="addTooltipProduct(product)"
              class="search-tooltip"
              :class="{ exist: findExistTooltips(product) }"
            >
              <live-search :title="product.title" />
            </li>
          </ul>
        </template>
      </div>
      <div @click="changeVisibility" class="shopping-cart">
        <shoppingCart />
        <div class="shopping-cart-counter">{{ cartProducts.length }}</div>
        <div v-show="cartProducts.length && isShow" class="shopping-cart-products">
          <template v-for="product in uniqueCartProducts" :key="product.name">
            <cart
              :title="product.title"
              :price="product.price"
              :image="product.image"
              :quantity="countCartProducts(product)"
            />
            </template>
        </div>
      </div>
    </div>

    <ul class="list-default flex">
      <li v-for="(product, index) in uniqueProducts" :key="index">
        <product-card
          :title="product.title"
          :price="product.price"
          :img-url="product.image"
          :count="product.count"
          @addToBasket="addToBasket(product)"
        />
      </li>
    </ul>
  </div>
</template>

<script>
import ProductCard from "./ProductCard";
import MyInput from "./MyInput";
import LiveSearch from "./liveSearch";
import Cart from "@/components/Cart";
import shoppingCart from "@carbon/icons-vue/es/shopping--cart/32";

export default {

  name: "ProductList",
  components: { Cart, ProductCard, MyInput, LiveSearch, shoppingCart },
  created() {
    setTimeout(async () => {
      const f = await fetch(`https://fakestoreapi.com/products`);
      this.newProducts = await f.json();
    }, 0);
    console.log(this.newProducts);
  },
  data() {
    return {
      cartProducts: [],
      tooltipProducts: [],
      newProducts: [],
      filteredProducts: [],
      products: [
        {
          title: "Стол Jim",
          price: 19999,
          image:
            "https://hoff.ru/upload/iblock/8c9/8c989500ee8639b9e3aa267b1888b4af.jpg",
          count: 30,
        },
        {
          title: "Стол Monaco",
          price: 17199,
          image:
            "https://hoff.ru/upload/iblock/af2/af2d37161f217d4f489373be7da4a7a0.jpg",
          count: 20,
        },
        {
          title: "Стол с двумя ящиками Кварт MD 768",
          price: 10999,
          image:
            "https://hoff.ru/upload/iblock/1cc/1ccb33b50e3e9cfa08629bf455e41855.jpg",
          count: 23,
        },
      ],
      isShow: false,
    };
  },
  methods: {
    addToBasket(product) {
      this.cartProducts.push(product);
    },
    addNewProduct(searchText) {
      this.filteredProducts = this.newProducts
        .filter((product) =>
          product.title.toLowerCase().includes(searchText.toLowerCase())
        )
        .forEach((product) => this.products.push(product));
    },
    addTooltipProduct(currentProduct) {
      this.filteredProducts = this.newProducts.filter((product) =>
        product.title.toLowerCase().includes(currentProduct.title.toLowerCase())
      );
      this.filteredProducts.forEach((product) => this.products.push(product));
    },
    findExistTooltips(currentProduct) {
      return this.products.includes(currentProduct);
    },
    validateInput(searchText) {
      this.tooltipProducts = this.newProducts.filter((product) =>
        product.title.toLowerCase().includes(searchText.toLowerCase())
      );
    },
    changeVisibility() {
      this.isShow = !this.isShow
    },
    countCartProducts(product) {
      let quantity = 0;
      this.cartProducts.forEach((p) => p === product ? quantity++ : null)
      return quantity
    },
  },
  computed: {
    uniqueCartProducts() {
      return new Set(this.cartProducts);
    },
    uniqueProducts() {
      return new Set(this.products);
    }
  }
};
</script>

<style scoped>
li {
  margin: 20px;
}
.top-information {
  align-items: center;
  margin-bottom: 22px;
}
h1 {
  margin-bottom: 0;
  margin-top: 0;
  margin-right: 20px;
}
.list-default {
  list-style: none;
  padding: 0;
}
.flex {
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  flex-wrap: wrap;
}
.top-information {
  justify-content: left;
  padding: 0 60px;
}
.search-wrapper {
  position: relative;
}
.search-tooltips {
  position: absolute;
  list-style: none;
  padding: 10px;
  display: flex;
  flex-wrap: wrap;
  background: #fff;
  border-radius: 10px;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  top: 20px;
  width: 350px;
}
.search-tooltip {
  padding: 5px 10px;
  margin: 5px;
  border: 1px solid gainsboro;
  border-radius: 10px;
  cursor: pointer;
  position: relative;
}
.exist:before,
.exist:after {
  content: "";
  width: 100%;
  height: 3px;
  display: block;
  background: red;
  transform: rotate(15deg);
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
.exist:after {
  transform: rotate(-15deg);
}
.shopping-cart-counter {
  position: absolute;
  top:20px;
  right: 0;
  background: rgb(221, 56, 56);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  color: #ffffff;
  width: 16px;
  height: 16px;
  border-radius: 50%;
}
</style>
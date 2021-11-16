<template>
    <div class="cart-product">
      <div class="cart-parameters">
        <div class="cart-parameters-image"><img :src="image" width="100" alt=""/></div>
        <div class="cart-parameters-title">{{ title }}</div>
      </div>
      <div class="cart-parameters space-between">
        <div class="cart-parameters-price">Цена: {{ price }}</div>
        <div class="cart-parameters-quantity">Количество: {{ quantity }}</div>
      </div>
    </div>
</template>

<script>
export default {
  name: "Cart",
  props: {
    title: {
      type: String,
      validator(value) {
        return value.length < 500;
      },
    },
    price: {
      type: Number,
      required: true,
    },
    image: {
      type: String,
      default: "https://tsum.by/images/no-photo.png",
    },
    quantity: {
      type: Number,
      default: 0,
    },
  }
}

</script>
<style>
.shopping-cart {
  margin-left: auto;
  position: relative;
}
.shopping-cart svg {
  cursor: pointer;
}
.shopping-cart-products {
  position: absolute;
  right: 0px;
  width: max-content;
  max-width: 350px;
  background: #fff;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  padding: 10px;
  border-radius: 10px;
}
.cart-product {
  padding-bottom: 10px;
  margin-bottom: 10px;
  border-bottom: 1px solid gainsboro;
}
.cart-parameters {
  display: flex;
}
.cart-parameters-title {
  padding: 0px 10px;
}
.space-between {
  justify-content: space-between;
}
</style>
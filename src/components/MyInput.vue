<template>
  <div class="flex">
    <input @input="$emit('check', searchText)" type="text" class="search" v-model="searchText" />
    <button @click="$emit('add', searchText)">Добавить</button>
  </div>
</template>

<script>

export default {
  data() {
    return {
      searchText: ""
    }
  },

}
</script>

<style scoped>
.my_input input {
  height: 25px;
  margin-left: 40px;
}
.flex {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}
button {
  background-color: rgb(221, 56, 56);
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 8px 14px;
  display: block;
  cursor: pointer;
  margin-left: 10px;
}
input {
  height: 25px;
}
</style>

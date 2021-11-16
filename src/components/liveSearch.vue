<template>
  {{ title.substring(0, 10) }}
</template>

<script>
export default {
  name: "LiveSearch",
  props: {
    title: {
      type: String
    }
  }
}
</script>
<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    msg: String,
  },
};
</script>

<style scoped>
h1 {
  text-align: center;
  margin: 40px 0 0;
}
</style>

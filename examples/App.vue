<template>
  <div>
    <button @click="clickHander">点击计算1+1</button>
  </div>
</template>

<script>
import vueCliLibDemo from "../src/main";

export default {
  methods: {
    clickHander() {
      alert(`1+2=${vueCliLibDemo.add(1, 2)}`);
    },
  },
};
</script>

<style></style>

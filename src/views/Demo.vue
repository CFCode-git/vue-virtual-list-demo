<template>
  <div class="demo">
    <virtual-list :data="data"/>
  </div>
</template>

<script>
import VirtualList from "./virtualList.vue";
export default {
  name: "demo",
  components: {
    VirtualList,
  },
  data() {
    const data = [];
    for (let i = 0; i < 10000; i++) {
      data.push({ value: i });
    }
    return {
      data
    };
  },
};
</script>

<style VirtualList lang="scss"></style>

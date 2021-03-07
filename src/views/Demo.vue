<template>
  <div class="demo">
    <virtual-list :data="data" :item-size-getter="itemSizeGetter" />
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
      data,
    };
  },
  methods: {
    itemSizeGetter(item) { // 根据 item 计算每一个高度
      return 30 + (item.value % 10);
    },
  },
};
</script>

<style VirtualList lang="scss"></style>

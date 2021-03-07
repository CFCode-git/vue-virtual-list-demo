<template>
  <div class="list-view" @scroll="handleScroll">
    <div class="list-view-phantom" :style="{ height: contentHeight }"></div>
    <div ref="content" class="list-view-content">
      <div
        class="list-view-item"
        :style="{ height: itemHeight + 'px' }"
        v-for="item in visibleData"
      >
        {{ item.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ListView",
  props: {
    data: {
      type: Array,
      required: true,
    },
    itemHeight: {
      type: Number,
      default: 30,
    },
  },
  computed: {
    contentHeight() {
      return this.data.length * this.itemHeight + "px";
    },
  },
  mounted() {
    this.updateVisibleData();
  },
  data() {
    return {
      visibleData: [],
    };
  },
  methods: {
    // 更新可见元素
    updateVisibleData(scrollTop) { 
      scrollTop = scrollTop || 0;
      const visibleCount = Math.ceil(this.$el.clientHeight / this.itemHeight);
      const start = Math.floor(scrollTop / this.itemHeight);
      const end = start + visibleCount;
      this.visibleData = this.data.slice(start, end);
      this.$refs.content.style.webkitTransform = `translate3d(0,${start *
        this.itemHeight}px,0`;
    },
    // 处理鼠标滚动事件
    handleScroll() {
      const scrollTop = this.$el.scrollTop;
      this.updateVisibleData(scrollTop);
    },
  },
};
</script>

<style lang="scss">
.list-view { /* 列表元素 */
  height: 400px;
  overflow: auto;
  position: relative;
  border: 1px solid #aaa;
}
.list-view-phantom { /* 不可见元素用于撑起整个列表, 使得列表的滚动条出现 */ 
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  z-index: -1;
}
.list-view-content { /* 列表的可见元素使用绝对定位 */
  left: 0;
  right: 0;
  top: 0;
  position: absolute;
}
.list-view-item {
  padding: 5px;
  color: #666;
  line-height: 30px;
  box-sizing: border-box;
}
</style>

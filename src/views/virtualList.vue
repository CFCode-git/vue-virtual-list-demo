<template>
  <div class="list-view" @scroll="handleScroll">
    <div class="list-view-phantom" :style="{ height: contentHeight + 'px' }"></div>
    <div ref="content" class="list-view-content" :style="{height:'100%'}">
      <div
        class="list-view-item"
        v-for="item in visibleData"
        :style="{ height: itemSizeGetter(item) + 'px' }"
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
    itemSizeGetter: {
      type: Function,
    },
  },
  computed: {
    // 计算整个虚拟列表的高度
    contentHeight() {
      const { data, itemSizeGetter } = this;
      let total = 0;
      for (let i = 0, j = data.length; i < j; i++) {
        total += itemSizeGetter.call(null, data[i], i);
      }
      return total;
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
      const start = this.findNearestItemIndex(scrollTop);
      const end = this.findNearestItemIndex(scrollTop + this.$el.clientHeight);
      this.visibleData = this.data.slice(start, Math.min(end + 1, this.data.length));
      this.$refs.content.style.webkitTransform = `translate3d(0, ${ this.getItemSizeAndOffset(start).offset }px, 0)`;
    },
    // 处理鼠标滚动事件
    handleScroll() {
      const scrollTop = this.$el.scrollTop;
      this.updateVisibleData(scrollTop);
    },
    // 计算可视区域的起始索引
    findNearestItemIndex(scrollTop) {
      const { data, itemSizeGetter } = this;
      let total = 0;
      for (let i = 0, j = data.length; i < j; i++) {
        const size = itemSizeGetter.call(null, data[i], i);
        // 从下标为0的项开始累加
        // 直到累加的长度 大于等于 scrollTop 或者
        // 说明这一项已经出现在可视区域内了
        total += size;
        if (total >= scrollTop || i === j - 1) {
          return i;
        }
      }
      // 最开始的时候 scrollTop 为 0 , 而第一项是有高度的
      return 0;
    },
    getItemSizeAndOffset(index) {
      // 通过索引计算,获取某个列表项相对于整个列表中的top
      const { data, itemSizeGetter } = this;
      let total = 0;
      for (let i = 0, j = Math.min(index, data.length - 1); i <= j; i++) {
        const size = itemSizeGetter.call(null, data[i], i);
        // 一旦 i = j
        // 说明 i 和 j 在同一位置, total不需要再加size了,即为top
        if (i === j) {
          return {
            offset: total,
            size,
          };
        }
        total += size;
      }
      return {
        offset: 0,
        size: 0,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.list-view {
  /* 列表元素 */
  height: 400px;
  overflow: auto;
  position: relative;
  color: #333;
  border: 1px solid #aaa;
}
.list-view-phantom {
  /* 不可见元素用于撑起整个列表, 使得列表的滚动条出现 */
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}
.list-view-content {
  /* 列表的可见元素使用绝对定位 */
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

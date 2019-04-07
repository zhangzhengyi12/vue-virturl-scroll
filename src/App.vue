<template>
  <div id="app">
    <div class="scroll-box" @scroll="onScrollHandle">
      <ul class="wrap" :style="`height:${wrapHeight}px`">
        <li
          class="item"
          v-for="(v,i) of renderList"
          :key="v.id"
          :style="`color:${v.color};top:${20 * v.id}px`"
        >{{v.id}}</li>
      </ul>
    </div>
  </div>
</template>
<script>
const viewportHeight = 400;
const viewportItemHeight = 20;
const viewportItemCount = 100000;
export default {
  data() {
    const list = Array(viewportItemCount)
      .fill(1)
      .map((v, i) => {
        const color = "#" + Math.floor(Math.random() * 256 ** 3).toString(16);
        const id = i;
        return {
          color,
          id
        };
      });
    return {
      list,
      renderList: [],
      wrapHeight: 0
    };
  },
  created() {
    this.wrapHeight = viewportItemHeight * viewportItemCount;
    this.renderList = this.calcluteRenderList(0);
  },
  methods: {
    calcluteRenderList(scrollTop) {
      const renderCount = viewportHeight / viewportItemHeight + 10;
      const renderTopCount = Math.floor(scrollTop / viewportItemHeight);
      return this.list.slice(renderTopCount, renderTopCount + renderCount);
    },
    onScrollHandle(e) {
      this.renderList = this.calcluteRenderList(e.target.scrollTop);
    }
  }
};
</script>
<style lang="stylus">
html, body
  padding 0
  margin 0
.scroll-box
  overflow-y scroll
  height 400px
  .wrap
    overflow hidden
    padding 0
    margin 0
    position relative
    .item
      height 20px
      position absolute
      list-style none
</style>

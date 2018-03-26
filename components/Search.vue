<template>
  <div class="search-box">
    <div class="ipt-box">
      <input type="text" class="ipt" placeholder="城市名称/拼音" @keydown="entry()" v-model="searchText" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data () {
    return {
      searchText: '',
      timer: {}
    }
  },
  methods: {
    entry () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      this.timer = setTimeout(() => {
        this.$emit('txtdata', this.searchText)
      }, 300)
    }
  }
}
</script>

<style lang='stylus' scoped>
@import '~common/stylus/css.styl';

.search-box
  height: 50px;
  background: $color-background;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  .ipt-box
    width: 80%;

    .ipt
      display: block;
      width: 100%;
      height: 40px;
      border: none;
      outline: none;
      border-radius: 20px;
      box-sizing: border-box;
      padding-left: 40px;
      line-height: 40px;
      font-size: $font-size-l;
</style>

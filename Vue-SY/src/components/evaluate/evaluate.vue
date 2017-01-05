<template>
  <div class="evaluate">
    <div class="evaluate-header">
      <span class="block recommend">{{desc.all}}<span class="count">57</span></span>
      <span class="block recommend">{{desc.satisfaction}}<span class="count">50</span></span>
      <span class="block tuCao">{{desc.displeasure}}<span class="count">7</span></span>
    </div>
    <div class="ev-switch">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  // const POSITIVE = 0;
  // const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      ratings: {
        type: Array,
        default () {
          return [];
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default () {
          return {
            all: '全部',
            satisfaction: '满意',
            displeasure: '不满意'
          };
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .evaluate
    .evaluate-header
      margin: 0 18px
      padding: 12px 0 18px 0
      font-size: 0
      border-1px(rgba(7, 17, 27, 0.1))
      .block
        display: inline-block
        font-size: 12px
        padding: 8px 12px
        margin-right: 8px
        &.recommend
          background: rgba(0, 160, 220, 0.2)
        &.tuCao
          background: rgba(77, 85, 93, 0.2)
    .ev-switch
      font-size: 0
      margin: 0 18px
      padding: 12px 0
      .icon-check_circle
        display: inline-block
        font-size: 24px
        color: rgb(147, 153, 159)
        line-height: 24px
        margin-right: 4px
        vertical-align: top
      .text
        display: inline-block
        font-size: 12px
        color: rgb(147, 153, 159)
        line-height: 24px
        vertical-align: top
</style>

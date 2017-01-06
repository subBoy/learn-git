<template>
  <div class="evaluate">
    <div class="evaluate-header">
      <span class="block recommend" @click="selected_fun(2, $event)" :class="{'selected': selectType === 2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span class="block recommend" @click="selected_fun(0, $event)" :class="{'selected': selectType === 0}">{{desc.satisfaction}}<span class="count">{{recommend}}</span></span>
      <span class="block tuCao" @click="selected_fun(1, $event)" :class="{'selected': selectType === 1}">{{desc.displeasure}}<span class="count">{{tuCao}}</span></span>
    </div>
    <div class="ev-switch">
      <span class="icon-check_circle" @click="onlyContent_fun($event)" :class="{'onlyContent': onlyContent}"></span>
      <span class="text">只看有内容的评价</span>
    </div>
    <comment :ratings="ratings"></comment>
  </div>
</template>

<script>
  import comment from 'components/comment/comment';
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
    },
    computed: {
      recommend () {
        let num = 0;
        this.ratings.forEach((rating) => {
          if (rating.rateType === 0) {
            num++;
          };
        });
        return num;
      },
      tuCao () {
        return this.ratings.length - this.recommend;
      }
    },
    methods: {
      selected_fun (type, event) {
        if (!event._constructed) {
          return;
        };
        this.$emit('ratingsType', type);
      },
      onlyContent_fun (event) {
        if (!event._constructed) {
          return;
        };
        this.$emit('onlyContent-fun');
      }
    },
    components: {
      comment
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
        line-height: 16px
        padding: 8px 12px
        margin-right: 8px
        color: rgb(77, 85, 93)
        .count
          display: inline-block
          margin-left: 2px
          line-height: 16px
          font-size: 8px
        &.recommend
          background: rgba(0, 160, 220, 0.2)
          &.selected
            color: #fff
            background: rgb(0, 160, 220)
        &.tuCao
          background: rgba(77, 85, 93, 0.2)
          &.selected
            color: #fff
            background: rgb(77, 85, 93)
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
        &.onlyContent
          color: #00b43c
      .text
        display: inline-block
        font-size: 12px
        color: rgb(147, 153, 159)
        line-height: 24px
        vertical-align: top
</style>

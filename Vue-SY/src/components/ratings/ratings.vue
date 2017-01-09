<template>
  <div class="ratings-wrapper">
    <div class="ratings-info">
      <div class="ratings-info-left">
        <p class="score">{{seller.score}}</p>
        <p class="desc">综合评分</p>
        <p class="rankRate">高于周边商家{{seller.rankRate}}%</p>
      </div>
      <div class="ratings-info-right"></div>
    </div>
    <split></split>
  </div>
</template>

<script>
  import split from 'components/split/split';
  import star from 'components/star/star';
  export default {
    props: {
      seller: {
        type: Object,
        default () {
          return {};
        }
      }
    },
    components: {
      split,
      star
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratings-wrapper
    .ratings-info
      display: flex
      padding: 18px 0
      .ratings-info-left
        flex: 0 0 137.5px
        width: 137.5px
        border-right: 1px solid rgba(7, 17, 27, 0.1)
        text-align: center
        .score
          font-size: 24px
          color: rgb(255, 153, 0)
          line-height: 28px
          margin-bottom: 6px
        .desc
          font-size: 12px
          line-height: 12px
          color: rgb(7, 17, 27)
          margin-bottom: 8px
        .rankRate
          font-size: 10px
          color: rgb(147, 153, 159)
          padding-bottom: 6px
      .ratings-info-right
        flex: 1
</style>

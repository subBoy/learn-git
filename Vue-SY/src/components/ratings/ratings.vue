<template>
  <div class="ratings-wrapper">
    <div class="ratings-info">
      <div class="ratings-info-left">
        <p class="score">{{seller.score}}</p>
        <p class="desc">综合评分</p>
        <p class="rankRate">高于周边商家{{seller.rankRate}}%</p>
      </div>
      <div class="ratings-info-right">
        <div class="ratings-att">
          <span class="title">服务态度</span>
          <star :size="36" :score="seller.serviceScore"></star>
          <span class="num">{{seller.serviceScore}}</span>
        </div>
        <div class="ratings-att">
          <span class="title">商品评分</span>
          <star :size="36" :score="seller.foodScore"></star>
          <span class="num">{{seller.foodScore}}</span>
        </div>
        <div class="delivery-time">
          <span class="title">送达时间</span>
          <span class="time">{{seller.deliveryTime}}分钟</span>
        </div>
      </div>
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
        @media only screen and (max-width:320px)
          flex: 0 0 120px
          width: 120px
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
        margin-left: 24px
        @media only screen and (max-width: 320px)
          margin-left: 6px
        .ratings-att
          font-size: 0
          margin-bottom: 8px
          .title
            display: inline-block
            vertical-align: top
            font-size: 12px
            line-height: 18px
            color: rgb(7, 17, 27)
            margin-right: 12px
          .star
            display: inline-block
            vertical-align: top
            &.star-36
            .star-item
              margin-right: 6px
          .num
            display: inline-block
            vertical-align: top
            font-size: 12px
            color: rgb(255, 153, 0)
            line-height: 18px
        .delivery-time
          font-size: 0
          .title
            display: inline-block
            vertical-align: top
            font-size: 12px
            line-height: 18px
            color: rgb(7, 17, 27)
            margin-right: 12px
          .time
            display: inline-block
            font-size: 12px
            color: rgb(147, 153, 159)
            line-height: 18px
</style>

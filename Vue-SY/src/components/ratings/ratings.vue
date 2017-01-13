<template>
  <div class="ratings-wrapper" ref="ratingsWrapper">
    <div class="ratings-content">
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
      <evaluate @ratingsType="ratingsType" @onlyContent-fun="onlyContent_fun" :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></evaluate>
      <div class="ratings-list">
        <ul>
          <li class="ratings-item border-1px" v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)">
            <div class="avatar">
              <img :src="rating.avatar">
            </div>
            <div class="info-content">
              <div class="rateTime">{{rating.rateTime | formatDate}}</div>
              <h3 class="title">{{rating.username}}</h3>
              <p class="info-evaluate">
                <star :size="24" :score="rating.score"></star>
                <span class="deliveryTime" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </p>
              <p class="txt" v-show="rating.text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend.length">
                <span class="icon" :class="{'icon-thumb_down': rating.rateType === 1, 'icon-thumb_up': rating.rateType === 0}"></span>
                <div class="recommend-list">
                  <span v-for="item in rating.recommend" class="recommend-item">{{item}}</span>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import split from 'components/split/split';
  import star from 'components/star/star';
  import evaluate from 'components/evaluate/evaluate';
  import {formatDates} from 'common/js/date';
  let ERR_OK = 0;
  let ALL = 2;
  export default {
    props: {
      seller: {
        type: Object,
        default () {
          return {};
        }
      }
    },
    data () {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: true
      };
    },
    created () {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.ratings = response.data;
          this.$nextTick(() => {
            this.ratingScroll = new BScroll(this.$refs.ratingsWrapper, {
              click: true
            });
          });
        }
      });
    },
    methods: {
      resetHeight () {
        this.$nextTick(() => {
          if (!this.ratingScroll) {
            this.ratingScroll = new BScroll(this.$refs.ratingsWrapper, {
              click: true
            });
          } else {
            this.ratingScroll.refresh();
          };
        });
      },
      ratingsType (type) {
        this.selectType = type;
        this.resetHeight();
      },
      onlyContent_fun () {
        this.onlyContent = !this.onlyContent;
        this.resetHeight();
      },
      needShow (type, text) {
        if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
      }
    },
    components: {
      split,
      star,
      evaluate
    },
    filters: {
      formatDate: function (time) {
        let date = new Date(time);
        return formatDates(date, 'yyyy-MM-dd hh:mm');
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl';
  .ratings-wrapper
    position: absolute
    top: 174px
    bottom: 0
    width: 100%
    overflow: hidden
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
    .ratings-list
      padding: 0 18px 18px 18px
      border-top: 1px solid (rgba(7, 17, 27, 0.1))
      .ratings-item
        display: flex
        padding: 18px 0
        border-1px(rgba(7, 17, 27, 0.1))
        .avatar
          flex: 0 0 28px
          margin-right: 12px
          width: 28px
          img
            width: 100%
            border-radius: 50%
        .info-content
          position: relative
          flex: 1
          .rateTime
            position: absolute
            top: 0
            right: 0
            line-height: 12px
            font-size: 10px
            font-weight: 200
            color: rgb(147, 153, 159)
          .title
            line-height: 12px
            font-size: 10px
            color: rgb(7, 17, 27)
            margin-bottom: 4px
          .info-evaluate
            font-size: 0
            margin-bottom: 6px
            .star
              display: inline-block
              vertical-align: top
              margin-right: 6px
            .deliveryTime
              display: inline-block
              vertical-align: top
              line-height: 12px
              font-size: 10px
              color: rgb(147, 513, 519)
              font-weight: 200
          .txt
            line-height: 18px
            font-size: 12px
            color: rgb(7, 17, 27)
            margin-bottom: 8px
          .recommend
            display: flex
            font-size: 0
            .icon
              display: inline-block
              flex: 0 0 12px
              font-size: 12px
              line-height: 16px
              width: 12px
              &.icon-thumb_down
                color: rgb(147, 153, 159)
              &.icon-thumb_up
                color: rgb(0, 160, 220)
            .recommend-list
              flex: 1
              .recommend-item
                display: inline-block
                line-height: 16px
                font-size: 9px
                color: rgb(147, 153, 159)
                padding: 0 6px
                margin-left: 8px
                border: 1px solid rgba(7, 17, 27, 0.1)
                border-radius: 1px
                margin-bottom: 4px
</style>

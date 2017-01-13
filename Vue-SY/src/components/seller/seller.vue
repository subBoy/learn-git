<template>
  <div class="seller-wrapper" ref="sellerWrapper">
    <div class="seller-content">
      <div class="seller-header">
        <h3 class="name">{{seller.name}}</h3>
        <div class="count">
          <star :size="36" :score="seller.serviceScore"></star>
          <span class="ratingCount">({{seller.ratingCount}})</span>
          <span class="sellCount">月售{{seller.sellCount}}单</span>
        </div>
        <div class="collection" @click="collectioned">
          <i class="icon-favorite" :class="{'active': collection}"></i>
          <p class="collectionTxt">{{collectionTxt}}</p>
        </div>
      </div>
      <div class="seller-acay">
        <ul class="seller-acay-list">
          <li class="seller-acay-item">
            <p class="title">起送价</p>
            <p class="content"><span class="count">{{seller.minPrice}}</span>元</p>
          </li>
          <li class="seller-acay-item">
            <p class="title">商家配送费</p>
            <p class="content"><span class="count">{{seller.deliveryPrice}}</span>元</p>
          </li>
          <li class="seller-acay-item">
            <p class="title">平均配送时间</p>
            <p class="content"><span class="count">{{seller.deliveryTime}}</span>分钟</p>
          </li>
        </ul>
        </div>
      <split></split>
      <div class="seller-active">
        <h3 class="title">公告与活动</h3>
        <p class="bulletin">{{seller.bulletin}}</p>
        <ul class="supports" v-if="seller.supports">
          <li class="support-item border-1px" v-for="(item, index) in seller.supports">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text" v-text="seller.supports[index].description"></span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="seller-real">
        <h3 class="title">商品实景</h3>
        <div class="real-pics" ref="realPics">
          <ul class="real-pics-list" ref="realPicsList">
            <li class="real-pics-item" v-for="pic in seller.pics"><img :src="pic" width="120" height="90"></li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="seller-infos">
        <h3 class="title">商家信息</h3>
        <ul class="seller-infos-list">
          <li class="seller-infos-item" v-for="item in seller.infos">{{item}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import {saveToLocal, loadFromLocal} from 'common/js/store';
  import star from 'components/star/star';
  import split from 'components/split/split';
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        collection: (() => {
          return loadFromLocal(this.seller.id, 'collection', false);
        })()
      };
    },
    computed: {
      collectionTxt () {
        return this.collection ? '已收藏' : '收藏';
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    watch: {
      'seller' () {
        this.$nextTick(() => {
          this._initScroll();
          this._initPics();
        });
      }
    },
    mounted () {
      this.$nextTick(() => {
        this._initScroll();
        this._initPics();
      });
    },
    methods: {
      _initPics () {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth + margin) * this.seller.pics.length + margin * 2;
          this.$refs.realPicsList.style.width = width + 'px';
          if (!this.picScroll) {
            this.picScroll = new BScroll(this.$refs.realPics, {
              scrollX: true,
              eventPassthrough: 'vertical'
            });
          } else {
            this.picScroll.refresh();
          }
        }
      },
      _initScroll () {
        if (!this.sellerScroll) {
          this.sellerScroll = new BScroll(this.$refs.sellerWrapper, {
            click: true
          });
        } else {
          this.sellerScroll.refresh();
        }
      },
      collectioned (event) {
        if (!event._constructed) {
          return;
        };
        this.collection = !this.collection;
        saveToLocal(this.seller.id, 'collection', this.collection);
      }
    },
    components: {
      star,
      split
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl';
  .seller-wrapper
    position: absolute
    top: 174px
    bottom: 0
    width: 100%
    overflow: hidden
    .seller-content
      .seller-header
        position: relative
        padding: 18px
        .name
          margin-bottom: 8px
          line-height: 14px
          font-size: 14px
          color: rgb(7, 17, 27)
        .count
          font-size: 0
          padding-bottom: 18px
          border-1px(rgba(7, 17, 27, 0.1))
          .star
            display: inline-block
            vertical-align: top
          .ratingCount
            display: inline-block
            vertical-align: top
            margin-left: 8px
            line-height: 18px
            font-size: 10px
            color: rgb(77, 85, 93)
          .sellCount
            display: inline-block
            vertical-align: top
            margin-left: 12px
            line-height: 18px
            font-size: 10px
            color: rgb(77, 85, 93)
        .collection
          position: absolute
          top: 18px
          right: 13px
          width: 40px
          text-align: center
          .icon-favorite
            line-height: 24px
            font-size: 24px
            color: #d4d6d9
            &.active
              color: rgb(240, 20, 20)
          .collectionTxt
            margin-top: 4px
            line-height: 10px
            font-size: 10px
            color: rgb(77, 85, 93)
      .seller-acay
        margin-bottom: 18px
        .seller-acay-list
          display: flex
          .seller-acay-item
            display: inline-block
            flex: 1
            border-left: 1px solid rgba(7, 17, 27, 0.1)
            text-align: center
            &:first-child
              border: none
            .title
              line-height: 10px
              font-size: 10px
              color: rgb(147, 153, 159)
              margin-bottom: 4px
            .content
              line-height: 24px
              color: rgb(7, 17, 27)
              font-weight: 200
              font-size: 10px
              .count
                display: inline-block
                font-size: 24px
                color: rgb(7, 17, 27)
                font-weight: 200
      .seller-active
        padding: 18px 18px 0
        .title
          margin-bottom: 8px
          font-size: 14px
          line-height: 14px
          color: #07111b
        .bulletin
          padding: 0 12px
          line-height: 24px
          font-size: 12px
          color: rgb(240, 20, 20)
          padding-bottom: 16px
          border-1px(rgba(7, 17, 27, 0.1))
        .supports
          .support-item
            padding: 16px
            border-1px(rgba(7, 17, 27, 0.1))
            &:last-child
              border-none()
            .icon
              display: inline-block
              vertical-align: top
              height: 16px
              width: 16px
              margin-right: 6px
              background-size: 16px 16px
              background-repeat: no-repeat
              &.decrease
                bg-img("decrease_4")
              &.discount
                bg-img("discount_4")
              &.guarantee
                bg-img("guarantee_4")
              &.invoice
                bg-img("invoice_4")
              &.special
                bg-img('special_4')
            .text
              display: inline-block
              vertical-align: top
              line-height: 16px
              font-size: 12px
              color: rgb(7, 17 ,27)
              font-weight: 200
      .seller-real
        padding: 18px 0 18px 18px
        .title
          margin-bottom: 12px
          font-size: 14px
          line-height: 14px
          color: #07111b
        .real-pics
          width: 100%
          height: 90px
          overflow: hidden
          .real-pics-list
            white-space: nowrap
            .real-pics-item
              display: inline-block
              margin-right: 6px
              width: 120px
              height: 90px
              :last-child
                margin: 0
      .seller-infos
        padding: 18px 18px 0 18px
        .title
          padding-bottom: 12px
          font-size: 14px
          line-height: 14px
          color: #07111b
          border-1px(rgba(7, 17, 27, 0.1))
        .seller-infos-list
          .seller-infos-item
            padding: 16px 12px
            line-height: 16px
            font-size: 12px
            font-weight: 200
            color: rgb(7, 17, 27)
            border-1px(rgba(7, 17, 27, 0.1))
            &:last-child
              border-none()
</style>

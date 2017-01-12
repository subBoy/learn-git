<template>
  <div class="seller-wrapper" ref="sellerWrapper">
    <div class="seller-content">
      <div class="seller-header">
        <h3 class="name">{{seller.name}}</h3>
        <div class="count">
          <star :size="36" :score="seller.serviceScore"></star>
          <span class="ratingCount">({{seller.ratingCount}})</span>
          <span class="sellCount">{{seller.sellCount}}</span>
        </div>
        <div class="collection" @click="collectioned">
          <i class="icon-favorite"></i>
          <p class="collectionTxt">{{collectionTxt}}</p>
        </div>
        <div class="seller-acay">
          <ul>
            <li>
              <p>起送价</p>
              <p><span>{{seller.minPrice}}</span>元</p>
            </li>
            <li>
              <p>商家配送费</p>
              <p><span>{{seller.deliveryPrice}}</span>元</p>
            </li>
            <li>
              <p>平均配送时间</p>
              <p><span>{{seller.deliveryTime}}</span>分钟</p>
            </li>
          </ul>
        </div>
      </div>
      <split></split>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
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
        collectionTxt: '收藏'
      };
    },
    created () {
      this.$nextTick(() => {
        this.sellerScroll = new BScroll(this.$refs.sellerWrapper, {
          click: true
        });
        if (this.seller.collection) {
          this.collectionTxt = '已收藏';
        } else {
          this.collectionTxt = '收藏';
        }
      });
    },
    methods: {
      collectioned () {
        if (this.seller.collection) {
          this.seller.collection = !this.seller.collection;
        }
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
  .seller-content
    .seller-header
      padding: 18px
</style>

<!-- 自己的实现方法 -->
<template>
  <div class="comment">
    <ul class="ratings-list" v-show="currentRatings && currentRatings.length">
      <li class="ratings-item border-1px border-none" v-for="rating in currentRatings">
        <p class="time">
          <span class="block">{{rating.rateTime | formatDate}}</span>
          <span class="user-info">
            <span class="block username">{{rating.username}}</span>
            <span class="block avatar">
              <img class="user-icon" :src="rating.avatar" alt="">
            </span>
          </span>
        </p>
        <p class="content">
          <span class="block icon" :class="{'icon-thumb_down': rating.rateType === 1, 'icon-thumb_up': rating.rateType === 0}"></span>
          <span class="block text">{{rating.text}}</span>
        </p>
      </li>
    </ul>
    <p class="not-ratings" v-show="currentRatings && !currentRatings.length">暂时没有符合要求的评价！！！</p>
  </div>
</template>

<script>
  import {formatDates} from 'common/js/date';
  const POSITIVE = 0;
  const NEGATIVE = 1;
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
      }
    },
    computed: {
      currentRatings () {
        let _ratings = [];
        this.ratings.forEach((rating) => {
          if (this.onlyContent) {
            if (rating.text !== '') {
              if (this.selectType === ALL) {
                _ratings.push(rating);
              } else if (this.selectType === POSITIVE) {
                if (rating.rateType === 0) {
                  _ratings.push(rating);
                }
              } else if (this.selectType === NEGATIVE) {
                if (rating.rateType === 1) {
                  _ratings.push(rating);
                }
              }
            }
          } else {
            if (this.selectType === ALL) {
              _ratings.push(rating);
            } else if (this.selectType === POSITIVE) {
              if (rating.rateType === 0) {
                _ratings.push(rating);
              }
            } else if (this.selectType === NEGATIVE) {
              if (rating.rateType === 1) {
                _ratings.push(rating);
              }
            }
          }
        });
        return _ratings;
      }
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
  @import '../../common/stylus/mixin';
  .comment
    padding: 0 18px
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    .ratings-item
      position: relative
      padding: 16px 0
      border-1px(rgba(7, 17, 27, 0.1))
      &:last-child
        border-none()
      .time
        padding-bottom: 6px
        font-size: 0
        .block
          display: inline-block
          margin-right: 10px
          line-height: 12px
          font-size: 10px
          color: rgb(147, 153, 159)
        .user-info
          display: inline-block
          float: right
          font-size: 0
          .block
            display: inline-block
            &.username
              margin-right: 6px
              line-height: 12px
              font-size: 10px
              color: rgb(147, 153, 159)
              vertical-align: top
            &.avatar
              width: 12px
              height: 12px
              overflow: hidden
              vertical-align: top
              .user-icon
                width: 100%
                height: 100%
                border-radius: 100%
      .content
        display: flex
        font-size: 0
        .block
          display: inline-block
          margin-right: 4px
          font-size: 12px
          line-height: 16px
          &.icon
            flex: 0 0 12px
            width: 12px
            &.icon-thumb_down
              color: rgb(147, 153, 159)
            &.icon-thumb_up
              color: rgb(0, 160, 220)
          &.text
            flex: 1
            color: rgb(7, 17, 27)
    .not-ratings
      padding: 18px
      font-size: 12px
      text-align: center
      color: rgb(147, 153, 159)
</style>

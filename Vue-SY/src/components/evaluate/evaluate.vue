<template>
  <div class="evaluate">
    <div class="evaluate-header">
      <span class="block recommend">{{desc.all}}<span class="count">57</span></span>
      <span class="block recommend">{{desc.satisfaction}}<span class="count">50</span></span>
      <span class="block tuCao">{{desc.displeasure}}<span class="count">7</span></span>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      ratings: {
        type: Array
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

</style>

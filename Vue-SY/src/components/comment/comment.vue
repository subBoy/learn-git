<template>
  <div class="comment">
    <ul>
      <li v-for="rating in ratings">
        <p><span>{{new Date(rating.rateTime).getFullYear()}}-{{new Date(rating.rateTime).getMonth()}}-{{new Date(rating.rateTime).getDate()}}</span><span>{{new Date(rating.rateTime).getHours()}}:{{new Date(rating.rateTime).getMinutes()}}</span></p>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    props: {
      ratings: {
        type: Array,
        default () {
          return [];
        }
      }
    },
    computed: {
      all_fun () {
        this.ratings.forEach((rating) => {

        });
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .comment
    padding: 0 18px
    border-top: 1px solid rgba(7, 17, 27, 0.1)
</style>

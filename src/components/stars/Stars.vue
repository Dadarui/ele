<template>
  <div class="star" :class="starType">
    <span class="star-item" v-for="item in starArr" :class="item"></span>
  </div>
</template>

<script>
  export default {
    props: {
      size: {
        type: Number
      },
      score: {
        type: Number
      }
    },
    computed: {
      starType () {
        return 'star-' + this.size
      },
      starArr () {
        let result = []
        let score = Math.floor(this.score * 2) / 2
        let hasHalf = score % 1 !== 0
        for (let i = 0; i < 5; i++) {
          if (i < Math.floor(score)) {
            result.push('on')
            continue
          }
          if (hasHalf && i === Math.floor(score)) {
            result.push('half')
            continue
          }
          result.push('off')
        }
        return result
      }
    }
  }
</script>

<style lang="less" scoped>
  @import '../../common/less/mixin.less';
  .star{
    font-size: 0;
    .star-item{
      display: inline-block;
      background-repeat: no-repeat;
    }
    &.star-48 {
      .star-item{
        width: 20px;
        height: 20px;
        margin-right: 22px;
        background-size: 20px 20px;
        .star-image('../../common/img/star48_')
      }
    }
    &.star-36{
      .star-item{
        width: 20px;
        height: 20px;
        margin-right: 22px;
        background-size: 20px 20px;
        .star-image('../../common/img/star36_')
      }
    }
    &.star-24{
      .star-item{
        width: 20px;
        height: 20px;
        margin-right: 22px;
        background-size: 20px 20px;
        .star-image('../../common/img/star24_')
      }
    }
  }
</style>

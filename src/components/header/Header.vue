<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar"/>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support">
          <ul ref="suppots" v-if="seller.supports">
            <li v-for="item in seller.supports.concat([seller.supports[0]])" :key="item.id">
              <span class="icon" :class="classMap[item.type]"></span>
              <span class="text">{{item.description}}</span>
            </li>
          </ul>
        </div>
        <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
    </div>
    <div class="bulletin-warpper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">
      {{seller.bulletin}}
    </span>
      <span class="icon-keyboard_arrow_right"></span>
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <Stars :size="48" :score="seller.score"></Stars>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li v-for="item in seller.supports" :key="item.id" class="support-list">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hiddenDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="150%" />
    </div>

  </div>
</template>

<script>
  import Stars from '../stars/Stars'
  import {mapActions, mapMutations} from 'vuex'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        classMap: this.$store.state.classMap,
        detailShow: false
      }
    },
    methods: {
      ...mapActions(['transitionEnd', 'transitionAgain', 'startHeaderTransition']),
      ...mapMutations(['RECEIVE_LENGTH']),
      showDetail () {
        this.detailShow = true
      },
      hiddenDetail () {
        this.detailShow = false
      }
    },
    watch: {
      seller(val) {
        if (val.supports) {
          this.RECEIVE_LENGTH(val.supports.length)
          this.$nextTick(function () {
            this.startHeaderTransition(this.$refs.suppots)
            this.transitionEnd({
              obj: this.$refs.suppots,
              callback: () => { this.transitionAgain({obj: this.$refs.suppots, modules: 'header'}) }
            })
          })
        }
      }
    },
    components: {
      Stars
    },
    mounted () {
    }
  }
</script>

<style lang="less" scoped>
  @import "../../common/less/mixin";

  @black: rgba(7, 17, 27, 0.5);
  @line-height: 12px;
  @font-weight: 200;

  .header {
    position: relative;
    overflow: hidden;
    color: #fff;
    background: @black;

    .content-wrapper {
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;
      .avatar {
        display: inline-block;
        vertical-align: top;
        img {
          border-radius: 4px;
        }
      }
      .content {
        display: inline-block;
        margin-left: 16px;
        .title {
          margin: 2px 0 8px 0;
        }
        .brand {
          display: inline-block;
          vertical-align: top;
          width: 30px;
          height: 18px;
          .bg-image("../../common/img/brand");
          background-size: 30px 18px;
          background-repeat: no-repeat;
        }
        .name {
          margin-left: 6px;
          font-size: 16px;
          line-height: 18px;
          font-weight: bold;
        }
        .description {
          margin-bottom: 10px;
          line-height: @line-height;
          font-size: @line-height;
          font-weight: @font-weight;
        }
        .support {
          height: 12px;
          overflow: hidden;
          li {
            margin-bottom: 4px;
            &:last-child {
              margin-bottom: 0;
            }
          }
          .icon {
            display: inline-block;
            vertical-align: top;
            width: 12px;
            height: 12px;
            margin-right: 4px;
            background-size: 12px 12px;
            background-repeat: no-repeat;
            .iconImg(1)
          }
          .text {
            line-height: @line-height;
            font-size: @line-height;
          }

        }
        .support-count {
          position: absolute;
          right: 12px;
          bottom: 14px;
          padding: 0 8px;
          height: 24px;
          text-align: center;
          border-radius: 12px;
          background: rgba(0, 0, 0, 0.2);
          .count {
            vertical-align: top;
            font-size: 10px;
            line-height: 24px;
          }
          .icon-keyboard_arrow_right {
            margin-left: 2px;
            font-size: 10px;
            line-height: 24px;
          }
        }
      }
    }
    .bulletin-warpper {
      position: relative;
      height: 28px;
      line-height: 28px;
      padding: 0 12px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      background: @black;
      .bulletin-title {
        display: inline-block;
        vertical-align: top;
        margin-top: 8px;
        width: 22px;
        height: 12px;
        .bg-image('../../common/img/bulletin');
        background-size: 22px 12px;
        background-repeat: no-repeat;
      }
      .bulletin-text{
        vertical-align: top;
        font-size: 10px;
        margin: 0 18px 0 4px;
      }
      .icon-keyboard_arrow_right {
        position: absolute;
        font-size: 10px;
        right: 10px;
        top: 10px;
      }
    }
    .background {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      filter: blur(10px);
    }
    .detail {
      position: fixed;
      z-index: 1000;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background: rgba(7, 17, 27, 0.8);
      transition: all 0.5s;
      &.fade-enter, .fade-enter-active{
        opacity: 0
      }
      .detail-wrapper {
        /*width: 100%;*/
        /*min-height: 100%;*/
        .detail-main {
          margin-top: 64px;
          padding-bottom: 64px;
          text-align: left;
          .name {
            line-height: 16px;
            font-size: 16px;
            text-align: center;
            font-weight: 700;
          }
          .star-wrapper {
            margin-top: 20px;
            text-align: center;
          }
          .title {
            display: flex;
            padding: 0 36px;
            margin: 28px auto 24px;
            .line{
              flex: 1;
              position: relative;
              top: -8px;
              border-bottom: 1px solid rgba(255,255,255,0.2);// 这样才出来
            }
            .text{
              padding: 0 12px;
              font-size: 12px;
              font-weight: 700;
            }
          }
          .supports {
            margin: 0 auto;
            padding: 0 36px;
            .support-list{
              padding: 0 12px;
              margin-bottom: 12px;
              font-size: 0;
              vertical-align: top;
              &:last-child{
                margin-bottom: 0;
              }
              .icon{
                display: inline-block;
                margin-right: 3px;
                padding-top: 4px;
                width: 16px;
                height: 16px;
                vertical-align: top;
                .iconImg(2);
                background-size: 16px 16px;
                background-repeat: no-repeat;
              }
              .text{
                font-size: 12px;
                line-height: 16px;
                font-weight: 200;
              }
            }
          }
          .bulletin{
            margin: 0 auto;
            padding: 0 36px;
            .content{
              padding: 0 12px;
              font-size: 12px;
              line-height: 24px;
              font-weight: 200;
            }
          }
        }
      }
      .detail-close{
        position: relative;
        margin: 0 auto;
        width: 32px;
        height: 32px;
        font-size: 20px;
      }
    }
  }
</style>

<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <div class="name">{{seller.name}}</div>
            <div class="star-wrapper">
              <star :size="36" :score="seller.score"></star>
            </div>
            <headline :headline=offers></headline>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <headline :headline=merchant></headline>
            <div class="bulletin">
              <div class="content">
                {{seller.bulletin}}
              </div>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from 'components/star/star'
  import headline from 'components/headline/headline'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        detailShow: false,
        discount: '优惠信息',
        proclamation: '商家公告'
      }
    },
    computed: {
      offers () {
        let discount = this.discount
        return discount
      },
      merchant () {
        let proclamation = this.proclamation
        return proclamation
      }
    },
    methods: {
      showDetail () {
        this.detailShow = true
      },
      hideDetail () {
        this.detailShow = false
      }
    },
    created () {
      this.classMap = [
        'decrease',
        'discount',
        'special',
        'invoice',
        'guarantee'
      ]
    },
    components: {
      star,
      headline
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  
  @import "../../common/stylus/mixin.styl";
  
  .header
    position: relative
    overflow: hidden
    color: #fff
    background: rgba(7, 17, 27, .5)
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        border-radius: 4px
        overflow: hidden
      .content
        display: inline-block
        vertical-align: top
        margin-left: 16px
        font-size: 14px
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block;
            vertical-align: top
            width: 30px
            height: 18px
            bg-image('./images/brand')
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            font-size: 16px
            line-height: 18px
            font-weight: bold
            margin-bottom: 8px
        .description
          margin-bottom: 10px
          font-size: 12px
          line-height: 12px
        .support
          font-size: 0
          .icon
            display: inline-block
            width: 12px
            height: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('./images/decrease_1')
            &.discount
              bg-image('./images/discount_1')
            &.invoice
              bg-image('./images/invoice_1')
            &.special
              bg-image('./images/special_1')
            &.guarantee
              bg-image('./images/guarantee_1')
          .text
            font-size: 10px
            vertical-align: top
            line-height: 12px;
      .support-count
        position: absolute
        right: 16px
        bottom: 16px
        width: 40px
        height: 24px
        line-height: 24px
        padding: 0 8px
        border-radius: 14px;
        background: rgba(0, 0, 0, .2)
        text-align: center
        .count
          vertical-align: top
          font-size: 10px
        .icon-keyboard_arrow_right
          margin-left: 2px
          line-height: 24px
          font-size: 10px
    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding: 0 22px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      background: rgba(7, 17, 27, .2)
      .bulletin-title
        display: inline-block
        margin-top: 8px;
        width: 22px
        height: 12px
        bg-image('./images/bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        vertical-align: top
        font-size: 10px
        margin: 0 4px
      .icon-keyboard_arrow_right
        position: absolute
        font-size: 10px
        right: 12px
        top: 8px
    .background
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
    .detail
      position: fixed
      top: 0
      left: 0
      width: 100%
      height: 100%
      overflow: auto
      z-index: 99
      backdrop-filter: blur(10px)
      background: rgba(7, 17, 27, .8)
      &.fade-enter-active, &.fade-leave-active
        transition: all .5s
      &.fade-enter, &.fade-leave-to
        opacity: 0
        background: rgba(7, 17, 27, 0)
      .detail-wrapper
        min-height: 100%
        width: 100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            font-size: 16px
            text-align: center;
            line-height: 16px
            font-weight: 700
          .star-wrapper
            margin-top: 16px
            padding: 2px 0
            text-align: center
          .supports
            width: 80%
            margin: 0 auto
            .support-item
              padding: 0 12px
              margin-bottom: 12px
              font-size: 0
              &:last-child
                margin-bottom: 0
              .icon
                display: inline-block
                width: 16px
                height: 16px
                background-size: 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-image('./images/decrease_2')
                &.discount
                  bg-image('./images/discount_2')
                &.invoice
                  bg-image('./images/invoice_2')
                &.special
                  bg-image('./images/special_2')
                &.guarantee
                  bg-image('./images/guarantee_2')
              .text
                height: 16px
                line-height: 16px
                vertical-align: top
                margin-left: 6px
                font-size: 12px
          .bulletin
            width: 80%
            margin: 0 auto
            .content
              padding: 0 12px
              font-size: 12px
              line-height: 24px
              font-weight: 200
      
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0
        clear: both
        font-size: 32px
</style>

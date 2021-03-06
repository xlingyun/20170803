<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="24" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>元
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active': favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item border-1px" v-for="(item, index) in seller.supports">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90" alt="">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title border-1px">商家信息</h1>
        <ul>
          <li class="info-item border-1px" v-for="info in seller.infos">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import star from 'components/star/star'
import split from 'components/split/split'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      favorite: false
    }
  },
  computed: {
    favoriteText () {
      return this.favorite ? '已收藏' : '收藏'
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
  watch: {
    'seller' () {
      this.$nextTick(() => {
        this._initScroll()
        this.__initPics()
      })
    }
  },
  mounted () {
    this.$nextTick(() => {
      this._initScroll()
      this.__initPics()
    })
  },
  methods: {
    toggleFavorite (event) {
      if (!event._constructed) {
        return
      }
      this.favorite = !this.favorite
    },
    _initScroll () {
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.seller, {
          click: true
        })
      } else {
        this.scroll.refresh()
      }
    },
    __initPics () {
      if (this.seller.pics) {
        let picWidth = 120
        let margin = 6
        let width = (picWidth + margin) * this.seller.pics.length - margin
        this.$refs.picList.style.width = width + 'px'
        this.$nextTick(() => {
          if (!this.picScroll) {
            this.picScroll = new BScroll(this.$refs.picWrapper, {
              scrollX: true,
              eventPassthrough: 'vertical'
            })
          } else {
            this.picScroll.refresh()
          }
        })
      }
    }
  },
  components: {
    star,
    split
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl'
.seller
  position: absolute
  top: 174px
  bottom: 0
  width: 100%
  overflow: hidden
  .overview
    position: relative
    padding: 18px
    .title
      font-size: 14px
      color: rgb(7, 17, 27)
      line-heigh: 14px
      margin-bottom: 8px
    .desc
      font-size: 0
      padding-bottom: 18px
      border-1px(rgba(7, 17, 27, .1))
      .star
        display: inline-block
        vertical-align: top
        margin-right: 5px
      .text
        display: inline-block
        vertical-align: top
        line-height: 18px
        font-size: 10px
        margin-right: 12px
        color: rgb(77, 85, 93)
    .remark
      display: flex
      padding-top: 18px
      .block
        flex: 1
        text-align: center
        border-right: 1px solid rgba(7, 17, 27, .1)
        &:last-child
          border: none
        h2
          margin-bottom: 4px
          font-size: 10px
          color: rgb(147, 153, 159)
          line-height: 10px
        .content
          line-height: 24px
          font-size: 10px
          color: rgb(7, 17, 27)
          .stress
            font-size: 24px
            font-weight: 200
    .favorite
      position: absolute
      right: 11px
      top: 18px
      width: 50px
      text-align: center
      .icon-favorite
        display: block
        margin-bottom: 4px
        line-height: 24px
        font-size: 24px
        color: #d4d6d9
        &.active
          color: rgb(240, 20, 20)
      .text
        line-height: 10px
        font-size: 10px
        color: rgb(77, 85, 93)
  .bulletin
    padding: 18px 18px 0
    .title
      margin-bottom: 8px
      font-size: 14px
      color: rgb(7, 17, 27)
      line-height: 14px
    .content-wrapper
      padding: 0 12px 16px
      border-1px(rgba(7, 17, 27, .1))
      .content
        font-size: 12px
        font-weight: 200
        color: rgb(240, 20, 20)
        line-height: 24px
    .supports
      .support-item
        padding: 16px
        border-1px(rgba(7, 17, 27, .1))
        font-size: 0
        &:last-child
          border-none()
        .icon
          display: inline-block
          vertical-align: top
          width: 16px
          height: 16px
          margin-right: 6px
          background-repeat: no-repeat
          background-size: contain
          &.decrease
            bg-image('./images/decrease_4')
          &.discount
            bg-image('./images/discount_4')
          &.invoice
            bg-image('./images/invoice_4')
          &.special
            bg-image('./images/special_4')
          &.guarantee
            bg-image('./images/guarantee_4')
        .text
          font-size: 12px
          font-weight: 200
          color: rgb(7, 17, 27)
          line-height: 16px
  .pics
    padding: 18px 0 18px 18px
    .title
      font-size: 14px
      color: rgb(7, 17, 27)
      line-height: 14px
      margin-bottom: 12px
    .pic-wrapper
      width: 100%
      overflow: hidden
      white-space: nowrap
      .pic-list
        font-size: 0
        .pic-item
          display: inline-block
          vertical-align: top
          margin-right: 6px
          &:last-child
            margin: 0
  .info
    padding: 18px 18px 0 18px
    color: rgb(7, 17, 27)
    .title
      padding-bottom: 12px
      border-1px(rgba(7, 17, 27, .1))
      font-size: 14px
      line-height: 14px
    .info-item
      padding: 16px 12px
      font-size: 12px
      font-weight: 200
      line-height: 16px
      border-1px(rgba(7, 17, 27, .1))
      &:last-child
        border-none()
</style>

<template>
  <div class="headline-title">
    <div class="line"></div>
    <div class="text">{{getContent}}</div>
    <div class="line"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: {
      headline: {
        type: String
      }
    },
    computed: {
      getContent () {
        let headline = this.headline
        return headline
      }
    }
  }
</script>
<style lang="stylus">
  .headline-title
    display: flex
    width: 80%
    margin: 30px auto 24px
    .line
      flex: 1
      position: relative
      top: -6px
      border-bottom: 1px solid rgba(255, 255, 255, .2)
    .text
      padding: 0 12px
      font-size: 14px
</style>

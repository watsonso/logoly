<style lang="stylus" scoped>
.pornhub
  display flex
  flex-direction  column
  align-items center
.box
    border 1px solid #333
    border-radius 10px
    padding 40px
    margin 40px 10px
    max-width 100%
    .editarea
        padding 20px
        text-align center
        font-size 60px
        font-weight 700
        .prefix
            color #fff
            padding 5px 5px

        .postfix
            color #000
            background-color #f90
            padding 5px 10px
            border-radius 7px

// customize things
.customize
  display flex
  justify-content space-around
  width 100%
  margin-bottom 50px
  .customize-color > div,
  .customize-misc > div
    padding 8px 0

// download and share buttons
.download-share
  display flex
  justify-content space-around
  width 80%
  & > div
      width 100px
      height 40px
      border-radius 3px
      line-height 40px
      text-align center
      cursor pointer
  .download
      color black
      background #f90
  .share
    color #fff
    background #1da1f2
</style>

<template>
  <div class="pornhub">
    <div
      class="box"
      v-tooltip="{
        content: 'Edit The Text to Create Your Own Logo',
        show: true,
        classes: 'tooltipClasses',
      }"
    >
      <div
        class="editarea"
        id="logo"
        :style="{ 'font-size': fontSize + 'px', 'background-color': transparentBgColor }"
      >
        <template v-if="!reverseHighlight">
          <span class="prefix" :style="{ color: prefixColor }" contenteditable spellcheck="false">{{
            prefixText
          }}</span>
          <span
            class="postfix"
            :style="{ color: suffixColor, 'background-color': postfixBgColor }"
            contenteditable
            spellcheck="false"
            >{{ suffixText }}</span
          >
        </template>
        <template v-else>
          <span
            class="postfix"
            :style="{ color: suffixColor, 'background-color': postfixBgColor }"
            contenteditable
            spellcheck="false"
            >{{ prefixText }}</span
          >
          <span class="prefix" :style="{ color: prefixColor }" contenteditable spellcheck="false">{{
            suffixText
          }}</span>
        </template>
      </div>
    </div>

    <div class="customize">
      <div
        class="customize-color"
        id="prefixColor"
        v-tooltip="{ content: 'Switch Color as You Like', show: true, classes: 'tooltipClasses' }"
      >
        <div>Prefix Text Color: &nbsp; <input type="color" v-model="prefixColor" /></div>
        <div>Suffix Text Color: &nbsp; <input type="color" v-model="suffixColor" /></div>
        <div>Suffix Background Color: &nbsp; <input type="color" v-model="postfixBgColor" /></div>
        <div>
          Transparent Background: &nbsp;
          <input type="checkbox" value="transparentBg" v-model="transparentBg" />
        </div>
      </div>

      <div class="customize-misc">
        <div>
          Font Size: <input type="range" min="30" max="200" v-model="fontSize" /> {{ fontSize }}px
        </div>
        <div>Reverse Highlight: <input type="checkbox" v-model="reverseHighlight" /></div>
      </div>
    </div>

    <div class="download-share">
      <div
        class="download"
        v-tooltip="{ content: 'Export Your Own Logo', show: true, classes: 'tooltipClasses' }"
        @click="download"
      >
        Export
      </div>

      <div class="share" @click="twitter"><i class="iconfont icon-twitter"></i> Tweet</div>
    </div>
  </div>
</template>

<script>
import domtoimage from 'dom-to-image'
const FileSaver = require('file-saver')

export default {
  name: 'pornhub',
  data() {
    return {
      prefixColor: '#ffffff',
      suffixColor: '#000000',
      postfixBgColor: '#ff9900',
      fontSize: '60',
      transparentBg: false,
      reverseHighlight: false,
      prefixText: 'Edit',
      suffixText: 'Me',
    }
  },
  mounted: function() {
    //   this.$tours['pornhub'].start()
  },
  methods: {
    download() {
      var node = document.getElementById('logo')

      domtoimage.toPng(node).then(function(blob) {
        FileSaver.saveAs(blob, 'logo.png')
      })
    },
    twitter() {
      this.$ga.event('social', 'action', 'twitter', 1)
      let url = 'https://logoly.pro'
      let text = encodeURIComponent(`Built with #LogolyPro, by @xiqingongzi ${url}`)
      window.open(`https://twitter.com/intent/tweet?text=${text}`)
    },
  },
  computed: {
    transparentBgColor() {
      if (this.transparentBg) {
        return 'transparent'
      } else {
        return '#000000'
      }
    },
  },
}
</script>

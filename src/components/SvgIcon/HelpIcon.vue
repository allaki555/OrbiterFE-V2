<template>
  <svg @mouseover="svgHover" @mouseout="svgHoverout" :class="[showedCls]" :style="styles" width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
    <path :fill="color" :fill-opacity="opacity" fill-rule="evenodd" clip-rule="evenodd" d="M10 2C8.94942 2 7.90914 2.20692 6.93853 2.60893C5.96793 3.01095 5.08601 3.6002 4.34315 4.34303C3.60028 5.08586 3.011 5.96774 2.60896 6.93829C2.20693 7.90885 2 8.94909 2 9.99961C2 11.0501 2.20693 12.0904 2.60896 13.0609C3.011 14.0315 3.60028 14.9134 4.34315 15.6562C5.08601 16.399 5.96793 16.9883 6.93853 17.3903C7.90914 17.7923 8.94942 17.9992 10 17.9992C12.1217 17.9992 14.1566 17.1564 15.6569 15.6562C17.1571 14.156 18 12.1212 18 9.99961C18 7.87798 17.1571 5.84325 15.6569 4.34303C14.1566 2.84281 12.1217 2 10 2ZM11.1111 14.2216C11.1111 14.5163 10.994 14.7989 10.7857 15.0073C10.5773 15.2156 10.2947 15.3327 10 15.3327C9.70531 15.3327 9.4227 15.2156 9.21433 15.0073C9.00595 14.7989 8.88889 14.5163 8.88889 14.2216C8.88889 13.927 9.00595 13.6444 9.21433 13.436C9.4227 13.2276 9.70531 13.1106 10 13.1106C10.2947 13.1106 10.5773 13.2276 10.7857 13.436C10.994 13.6444 11.1111 13.927 11.1111 14.2216ZM8.34222 7.30108C8.57778 6.8531 9.18489 6.44423 10 6.44423C11.1404 6.44423 11.7778 7.18642 11.7778 7.7775C11.7778 8.36858 11.1404 9.11077 10 9.11077C9.76425 9.11077 9.53816 9.20441 9.37146 9.3711C9.20476 9.53779 9.11111 9.76388 9.11111 9.99961V10.8885C9.11111 11.1242 9.20476 11.3503 9.37146 11.517C9.53816 11.6837 9.76425 11.7773 10 11.7773C10.2357 11.7773 10.4618 11.6837 10.6285 11.517C10.7952 11.3503 10.8889 11.1242 10.8889 10.8885V10.788C12.3396 10.4565 13.5556 9.33031 13.5556 7.7775C13.5556 5.91448 11.8044 4.66654 10 4.66654C8.62844 4.66654 7.35556 5.35539 6.768 6.47623C6.71043 6.57986 6.67418 6.69396 6.66137 6.81182C6.64856 6.92967 6.65946 7.0489 6.69343 7.16247C6.72739 7.27605 6.78373 7.38169 6.85913 7.47317C6.93454 7.56465 7.02748 7.64012 7.13249 7.69514C7.2375 7.75017 7.35246 7.78363 7.47059 7.79356C7.58873 7.80349 7.70766 7.78969 7.82038 7.75297C7.93311 7.71625 8.03735 7.65735 8.12696 7.57975C8.21658 7.50214 8.28977 7.40739 8.34222 7.30108Z" />
  </svg>
</template>

<script>
const hoverLightOpacity = 0.8
const hoverDarkOpacity = 0.6

export default {
  name: 'HelpIcon',
  props: {
    size: {
      type: String,
      required: false,
      default: 'lg' // xs lg sm
    },
    lightOpacity: {
      type: Number,
      required: false,
      default: 0.4
    },
    darkOpacity: {
      type: Number,
      required: false,
      default: 0.4
    }
  },
  data() {
    return {
      isHoverSvg: false,
    }
  },
  computed: {    
    styles() {
      const styles = {}
      if (this.$attrs.width) {
        styles.width = this.$attrs.width
      }
      if (this.$attrs.height) {
        styles.height = this.$attrs.height
      }
      return styles
    },
    showedCls() {
      return `svg-icon-${this.size}`
    },
    showedIconName() {
      return `${this.$store.state.themeMode}-help`
    },
    isLightMode() {
      return this.$store.state.themeMode === 'light'
    },
    color() {
      return this.isLightMode ? `rgba(51, 51, 51)` : `rgba(255, 255, 255)`
    },
    opacity() {
      if (this.isLightMode) {
        return this.isHoverSvg ? hoverLightOpacity : this.lightOpacity
      }
      return this.isHoverSvg ? hoverDarkOpacity : this.darkOpacity
    },
  },
  methods: {
    svgHover() {
      this.isHoverSvg = true
    },
    svgHoverout() {
      this.isHoverSvg = false
    }
  }
}
</script>

<style scoped lang="scss">
.svg-icon-xs {
  width: 40px;
  height: 40px;
}
.svg-icon-lg {
  width: 20px;
  height: 20px;
}
.svg-icon-sm {
  width: 16px;
  height: 16px;
}
.help-svg-box {
  display: inline-block;
}
</style>

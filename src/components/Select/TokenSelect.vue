<template>
  <div ref="ctx" class="ob-select-box" @click="showSelectDialog">
    <div style="display: flex; justify-content: center; align-items: center">
      <slot class="prfix" name="prefix">
        <template v-if="selectedItem.icon">
          <img
            v-if="selectedItem.iconType === 'img'"
            :src="selectedItem.icon"
            class="select-item-icon"
            alt=""
          />
          <svg-icon
            v-else
            class="select-item-icon"
            :iconName="selectedItem.icon"
          ></svg-icon>
        </template>
      </slot>
      <span class="selected-label">{{
        (datas.find((v) => v.value == value) || {}).label || ''
      }}</span>
    </div>

    <SvgIconThemed />
    <div
      ref="dialog"
      v-if="!isMobile"
      class="dialog"
      :style="{ display: dialogVisible ? 'block' : 'none' }"
    >
      <div
        v-for="item in datas"
        @click="selectItem(item)"
        :key="item.value"
        :class="['select-item', { selected: item.value == value }]"
      >
        <template v-if="item.icon">
          <img
            v-if="item.iconType === 'img'"
            :src="item.icon"
            class="select-item-icon"
            alt=""
          />
          <svg-icon
            v-else
            class="select-item-icon"
            :iconName="item.icon"
          ></svg-icon>
        </template>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { SvgIconThemed } from '../'
import { mapState } from 'vuex'

export default {
  name: 'TokenSelect',
  components: { SvgIconThemed },
  props: {
    datas: {
      type: Array,
      required: true,
    },
    value: {
      // eslint-disable-next-line vue/require-prop-type-constructor
      type: String | Number,
      required: true,
    },
  },
  data() {
    return {
      dialogVisible: false,
    }
  },
  computed: {
    ...mapState(['isMobile']),
    selectedItem() {
      return this.datas.find((v) => v.value == this.value) || {}
    },
  },
  methods: {
    showSelectDialog() {
      if (!this.isMobile) {
        this.dialogVisible = true
      } else {
        this.$emit('show')
      }
    },
    selectItem(item) {
      setTimeout(() => {
        this.dialogVisible = false
      }, 0)
      this.$emit('input', item.value)
    },
    handlerDialogOutsideClick(e) {
      if (this.dialogVisible) {
        const dialog = this.$refs.dialog
        const ctx = this.$refs.ctx
        let cur = e.target
        let hasFind = false
        while (cur && !hasFind) {
          if (cur === dialog || cur === ctx) {
            hasFind = true
          }
          cur = cur.parentElement
        }
        !hasFind && (this.dialogVisible = false)
      }
    },
  },
  mounted() {
    !this.isMobile &&
      document.addEventListener('click', this.handlerDialogOutsideClick)
  },
  unmounted() {
    !this.isMobile &&
      document.removeEventListener('click', this.handlerDialogOutsideClick)
  },
}
</script>

<style scoped lang="scss">
.ob-select-box {
  border-radius: 1.2rem;
  width: 100%;
  height: 4rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  position: relative;
  .prefix,
  .suffix,
  .select-item-icon {
    width: 2rem;
    height: 2rem;
  }
  .selected-label {
    margin: 0 0.4rem;
    font-weight: 700;
    font-size: 1.6rem;
    line-height: 2.4rem;
  }
  .dialog {
    position: absolute;
    left: 0;
    top: 4rem;
    border-radius: 1.2rem;
    min-width: 20rem;
    padding: 1rem 0;
    z-index: 100;
    .select-item {
      height: 4rem;
      display: flex;
      align-items: center;
      cursor: pointer;
      padding-left: 1.8rem;
      font-weight: 700;
      font-size: 1.6rem;
      line-height: 2.4rem;
      .select-item-icon {
        margin-right: 0.4rem;
      }
    }
  }
}
</style>

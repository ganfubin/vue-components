<template>
  <div :class="_clas" :style="styles" @click="clickHandler">
    <button type="button" :class="!option ? cssPrefix + 'select-placeholder':''" >{{option?option.label:placeholder}}</button>
    <select v-if="option" @focus="clickHandler" :value="value">
      <option v-for="item in options" :value="item.value" >{{item.label}}</option>
    </select>
  </div>
</template>

<script>
import Vue from 'vue'
import {Actionsheet, ActionsheetItem} from '../actionsheet'
import { cssPrefix } from 'utils/variable.js'
import { base, input } from 'utils/mixins.js'

export default {
  mixins: [base, input],
  props: {
    options: {
      type: Array
    }
  },
  computed: {
    _clas () {
      return [cssPrefix + 'select', this.clas]
    }
  },
  created () {
    this.optionUpdate(this.value)
  },
  destroyed () {
    if (this.popup) {
      this.popup.open = false
      setTimeout(() => {
        this.popup.$destroy()
        this.popup = null
      }, 2000)
    }
  },
  methods: {
    clickHandler () {
      let select = this
      let node = document.createElement('div')
      document.body.appendChild(node)
      /* eslint-disable no-new */
      this.popup = new Vue({
        el: node,
        template: '<actionsheet :open="open" :value="value" @on-close="closeHandler" @on-menu="menuHandler"><actionsheet-item v-for="item in options" :value="item.value" :disabled="item.disabled">{{item.label}}</actionsheet-item></actionsheet>',
        components: { Actionsheet, ActionsheetItem },
        data: {
          options: this.options,
          open: false,
          value: this.value
        },
        mounted () {
          this.open = true
        },
        destroyed () {
          document.body.removeChild(this.$el)
        },
        methods: {
          closeHandler () {
            this.open = false
            setTimeout(() => {
              this.$destroy()
            }, 1000)
          },
          menuHandler (val) {
            select.$emit('on-change', val)
          }
        }
      })
    },
    optionUpdate (val) {
      let option = null
      this.options.forEach((item) => {
        if (item.value === val) {
          option = item
        }
      })
      this.option = option
    }
  },
  watch: {
    value (val) {
      this.optionUpdate(val)
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      selected: false,
      option: null
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  @import '~styles/mixins.scss';
  .#{$css-prefix}{
    &select{
      position:relative;
      height:2.6rem;
      select{
        display:none;
      }
      button{
        border: 0;
        background-color: transparent;
        font-size: inherit;
        text-align: inherit;
        height: 100%;
        outline: none;
        box-sizing: border-box;
        width: 100%;
        padding:0;
        z-index:1;
      }
      &-placeholder{
        color: #999;
      }
    }
  }
</style>

<template>
  <router-link :class="_clas" :style="styles" :to="href">
    <slot></slot>
  </router-link>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
import { base } from 'utils/mixins.js'
export default {
  mixins: [base],
  computed: {
    _clas () {
      return [cssPrefix + 'link', this.active ? cssPrefix + 'link-active' : '', this.clas]
    }
  },
  props: {
    href: {
      type: [String, Object]
    },
    active: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  @import '~styles/mixins.scss';
  .#{$css-prefix}{
    &link{
      color:$primary-color;
      text-decoration: none;
      box-sizing: border-box;
      cursor: default;
      &-active{
        @include active;
      }
    }
  }
</style>

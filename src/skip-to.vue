<template>
  <a class="vue-skip-to" :href="to" :tabindex="tabindex">
    <slot>{{ text }}</slot>
  </a>
</template>

<script>
export default {
  name: 'VueSkipTo',

  props: {
    text: {
      type: String,
      default: 'Skip to main content'
    },
    to: {
      type: String,
      default: '#main'
    },
    tabindex: {
      type: [Number, null],
      default: null
    }
  },

  mounted () {
    this.init()
  },

  methods: {
    init () {
      window.addEventListener('hashchange', () => {
        this.focusElement(location.hash.substring(1))
      }, false)

      if (location.hash && location.hash.substring(1)) {
        this.focusElement(location.hash.substring(1))
      }
    },

    focusElement (id) {
      if (!id) return
      let element = window.document.getElementById(id)
      if (element) {
        if (!/^(a|select|input|button|textarea)/i.test(element.tagName.toLowerCase())) {
          element.setAttribute('tabindex', -1)
        }
        element.focus()
      }
    }
  }
}
</script>

<style>
.vue-skip-to {
  position: absolute;
  left: -10000px;
  top: 0;
  z-index: 1000;
  min-width: 1px;
  min-height: 1px;
  overflow: hidden;
  padding: 8px 10px;
  color: #fff
}

.vue-skip-to:focus {
  background-color: #800000;
  left: 0;
}
</style>

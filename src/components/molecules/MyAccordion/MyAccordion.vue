<template>
  <div :class="$style.accordion">
    <div v-bind:class="[$style.accordion__header, { open: visible }]" v-on:click="onClick">
      <slot name="header">
        <MyButton>accordion default title</MyButton>
      </slot>
    </div>
    <transition v-on:before-enter="onBeforeEnter" v-on:enter="onEnter" v-on:before-leave="onBeforeLeave" v-on:leave="onLeave">
      <div v-show="visible" :class="$style.accordion__body">
        <slot>
          <ul>
            <li>accordion default contents</li>
          </ul>
        </slot>
      </div>
    </transition>
  </div>
</template>

<script>
import MyButton from '@/components/atoms/MyButton/MyButton.vue'
export default {
  name: 'MyAccordion',
  props: {
    'isVisible': {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      visible: this.isVisible
    }
  },
  components: {
    MyButton
  },
  methods: {
    onClick () {
      this.visible = !this.visible
    },
    onBeforeEnter (el) {
      el.style.height = 0
    },
    onEnter (el, done) {
      el.style.height = el.scrollHeight + 'px'
    },
    onBeforeLeave (el) {
      el.style.height = el.scrollHeight + 'px'
    },
    onLeave (el, done) {
      el.style.height = 0
    }
  }
}
</script>
<style lang="scss" module>
.accordion {
  overflow: hidden;

  &__body {
    overflow: hidden;
    transition: opacity 0.5s, height 0.5s ease-out;
    opacity: 1;
    &:global(.v-enter), &:global(.v-leave-to) {
      opacity: 0;
    }
  }
}

</style>

<template>
  <label
    class="md-check"
    :class="{
      'is-disabled': disabled,
      'is-checked': isChecked
    }"
    @click="$_onClick"
  >
    <div class="md-check-icon">
      <md-icon :name="currentIcon" :size="size" />
    </div>
    <div class="md-check-label" v-if="$slots.default || label">
      <slot>{{ label }}</slot>
    </div>
  </label>
</template>

<script>
export default {
  name: 'md-check',

  components: {
    [Icon.name]: Icon,
  },

  props: {
    name: {
      type: [Boolean, String],
      default: true,
    },
    value: {
      type: [Boolean, String],
      default: false,
    },
    size: {
      type: String,
      default: 'md',
    },
    icon: {
      type: String,
      default: 'circle-right',
    },
    iconInverse: {
      type: String,
      default: 'circle',
    },
    label: {
      type: String,
      default: '',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },

  computed: {
    isChecked() {
      return this.value === this.name || (this.rootGroup && this.rootGroup.value.indexOf(this.name) !== -1)
    },
    currentIcon() {
      return this.isChecked ? this.icon : this.iconInverse
    },
  },

  inject: {
    rootGroup: {default: null},
  },

  methods: {
    $_onClick() {
      if (this.disabled) {
        return
      }

      if (typeof this.name === 'boolean') {
        this.$emit('input', !this.value)
      } else if (this.isChecked) {
        this.$emit('input', '')
        if (this.rootGroup) {
          this.rootGroup.uncheck(this.name)
        }
      } else {
        this.$emit('input', this.name)
        if (this.rootGroup) {
          this.rootGroup.check(this.name)
        }
      }
    },
  },
}


<style lang="stylus">
.md-check
  display flex
  align-items flex-start
  line-height 1.5
  margin-top v-gap-sm
  margin-bottom v-gap-sm
  &.is-checked
    .md-check-icon
      color check-color
  &.is-disabled
    .md-check-icon
    .md-check-label
      color color-text-disabled

.md-check-icon
  position relative
  flex-shrink 0
  top 0.75em
  line-height 0
  transform translateY(-50%)

.md-check-label
  margin-left h-gap-sm
  font-size inherit
</style>
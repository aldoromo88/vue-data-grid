<template lang="pug">
.filter(:class="{'filter--active': isActive}")
  img.square(src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7")
  .button(@click="onActive")
    icon.icon--filter(symbol="filter")
  transition(name="fade")
    .panel(v-if="isActive")
      .control
        p.control-label SORT
        round-checkbox.sortOption(v-for="(val, key) in sort", :label="key", :value="sort[key]", :isBlock="false", @input="onSortCheck(key, $event)")
      .control
        p.control-label LIMIT RANGE
        multi-range-slider(:min="bound[0]", :max="bound[1]", :value="range", @input="onRangeChange")
</template>

<script>
import RoundCheckbox from 'components/RoundCheckbox'
import MultiRangeSlider from 'components/MultiRangeSlider'
import Icon from 'components/Icon'

export default {
  props: {
    isActive: Boolean,
    bound: Array,
    range: Array
  },
  data () {
    return {
      sort: {
        ascending: false,
        descending: false
      }
    }
  },
  components: {
    RoundCheckbox,
    MultiRangeSlider,
    Icon
  },
  methods: {
    onActive () {
      this.$emit('change', !this.isActive)
    },
    onSortCheck (key, value) {
      let order = 0
      this.sort[key] = value
      if (value) {
        for (let prop in this.sort) {
          if (prop !== key) {
            this.sort[prop] = false
          }
        }
      }
      if (this.sort.ascending) {
        order = 1
      } else if (this.sort.descending) {
        order = -1
      }
      this.$emit('sort', order)
    },
    onRangeChange (value) {
      this.$emit('range', value)
    }
  }
}

</script>

<style lang="sass" scoped>
@import "../sass/variables"
@import "../sass/utils"
@import "../sass/transition"

.filter
  position: absolute
  top: 0
  right: 0
  height: 100%
  z-index: $popup-index
  cursor: pointer
  transition: background $medium
  width: 3em // fix firefox issue

.filter--active
  background: $cell-color
  box-shadow: $half-shadow

.button
  position: absolute
  top: 0
  left: 0
  right: 0
  bottom: 0
  text-align: center

.square
  display: block
  height: 100%
  width: auto

.icon--filter
  width: 1.6em
  height: 1.6em
  margin-top: 50%
  transform: translateY(-50%)

.panel
  position: absolute
  top: 100%
  right: 0
  width: 240px
  padding: 0.5em 1em
  background: $cell-color
  box-shadow: $shadow

.control
  @extend %clearfix
  padding: 0.5em 0

.control-label
  margin: 0.5em 0
  font-weight: 900

.sortOption
  &:first-of-type
    float: left
  &:last-of-type
    float: right
</style>

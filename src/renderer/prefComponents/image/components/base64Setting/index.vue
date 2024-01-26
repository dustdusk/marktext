<template>
  <section class="base64-setting">
    <h5>Base64 Transfer setting</h5>
    <text-box description="Max image size (KB)" :input="base64Setting.maxSize"
      :regexValidator="/^[1-9]\d*$/" :defaultValue="maxSize"
      :onChange="value => onSelectNumChange('maxSize', value)"></text-box>
    <text-box description="Max image Width" :input="base64Setting.maxWidth"
      :regexValidator="/^[1-9]\d*$/" :defaultValue="maxWidth"
      :onChange="value => onSelectNumChange('maxWidth', value)"></text-box>
    <text-box description="Max image Height" :input="base64Setting.maxHeight"
      :regexValidator="/^[1-9]\d*$/" :defaultValue="maxHeight"
      :onChange="value => onSelectNumChange('maxHeight', value)"></text-box>
    <range
        description="Compression quality"
        :value="base64Setting.quality"
        :min="0"
        :max="100"
        :step="5"
        :onChange="value => onSelectChange('quality', value)"
      ></range>
    <bool description="Keep image ratio"
      more="When selecting 'maintain aspect ratio,' the system will ensure the aspect ratio of the image and make sure that neither the width nor the height exceeds the specified dimensions."
      :bool="base64Setting.keepRatio"
      :onChange="value => onSelectChange('keepRatio', value)"></bool>
  </section>
</template>

<script>
import { mapState } from 'vuex'
import Bool from '@/prefComponents/common/bool'
import TextBox from '@/prefComponents/common/textBox'
import Range from '@/prefComponents/common/range'

export default {
  components: {
    Bool,
    TextBox,
    Range
  },
  data () {
    return {
      base64Setting: {
        maxSize: '0',
        maxWidth: '0',
        maxHeight: '0',
        quality: 0,
        keepRatio: true
      }
    }
  },
  computed: {
    ...mapState({
      maxSize: state => String(state.preferences.base64Setting.maxSize || 500),
      maxWidth: state => String(state.preferences.base64Setting.maxWidth || 800),
      maxHeight: state => String(state.preferences.base64Setting.maxHeight || 600),
      quality: state => state.preferences.base64Setting.quality || 70,
      keepRatio: state => state.preferences.base64Setting.keepRatio
    })
  },
  created () {
    this.$nextTick(() => {
      this.base64Setting = {
        maxSize: this.$store.state.preferences.base64Setting.maxSize + '',
        maxWidth: this.$store.state.preferences.base64Setting.maxWidth + '',
        maxHeight: this.$store.state.preferences.base64Setting.maxHeight + '',
        quality: this.$store.state.preferences.base64Setting.quality,
        keepRatio: this.$store.state.preferences.base64Setting.keepRatio
      }
    })
  },
  methods: {
    onSelectNumChange (type, value) {
      // TODO deep set
      this.base64Setting[type] = parseInt(value)
      this.setBase64Setting()
    },
    onSelectChange (type, value) {
      // TODO deep set
      this.base64Setting[type] = value
      this.setBase64Setting()
    },
    setBase64Setting () {
      const base64Setting = {
        maxSize: parseInt(this.base64Setting.maxSize),
        maxWidth: parseInt(this.base64Setting.maxWidth),
        maxHeight: parseInt(this.base64Setting.maxHeight),
        quality: this.base64Setting.quality,
        keepRatio: this.base64Setting.keepRatio
      }
      this.$store.dispatch('SET_SINGLE_PREFERENCE', { type: 'base64Setting', value: base64Setting })
    }
  }
}
</script>

<style scoped>
</style>

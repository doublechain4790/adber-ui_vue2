<script lang="jsx">
import { t } from '@adber/adber-ui/src/locale'
import InputNumber from './index'
export default {
  name: 'AdInputNumberRange',
  components: {
    InputNumber
  },
  model: {
    prop: 'params',
    event: 'update:inputNumberRange'
  },
  props: {
    params: {
      type: Object,
      required: true
    },
    // 数值精度
    precision: {
      type: Number,
      default: 0
    },
    // 前缀 min prop
    propStart: {
      type: String
    },
    // 后缀 max prop
    propEnd: {
      type: String
    },
    // 默认prop
    prop: {
      type: String,
      default: ''
      // 若不定义 propStart  propEnd 则为 最小值(`${prop}Start`)   最大值(`${prop}End`)
    },
    // 最小值多语言
    placeholderStart: {
      type: String,
      default: 'adb.el.input.min'
    },
    // 最大值多语言
    placeholderEnd: {
      type: String,
      default: 'adb.el.input.max'
    },
    // 自定义样式(最大值/最小值)
    itemStyle: {
      type: String,
      default: ''
    },
    // 自定义 宽(最大值/最小值)
    itemWidth: {
      type: String,
      default: '100px'
    }
    /*
    // 前置展示
    prependVisible: {
      type: Boolean,
      default: true
    },
    // 后置展示
    appendVisible: {
      type: Boolean,
      default: true
    } */
  },
  render () {
    const { prepend, append, controlsPosition, min, max, ...props } = this.$attrs
    const { inputNumberSize, params, placeholderStart, placeholderEnd, precision, local_propStart, local_propEnd, localStyle } = this
    const _controlsPosition = controlsPosition || 'right'
    const $slots = this.$slots

    const _prepend = prepend ? <span class='ad-input-number-range_addon prepend'>{prepend}</span> : ''
    const _append = append ? <span class='ad-input-number-range_addon append'>{append}</span> : ''
    return <div class={`ad-input-number-range ad-input-number-range--${inputNumberSize}`}>
      {$slots.prepend || _prepend}
      <InputNumber
        class='ad-input-number-range_start'
        attrs={props}
        min={min}
        max={params[local_propEnd] ?? max}
        precision={precision}
        size={inputNumberSize}
        controlsPosition={_controlsPosition}
        placeholder={t(placeholderStart)}
        style={localStyle}
        value={params[local_propStart]}
        onChange={this.onChangeStart}
        onBlur={(event) => this.$emit('blur', event, local_propStart)}
      />
      <span class='ad-input-number-range_line'>-</span>
      <InputNumber
        class='ad-input-number-range_end'
        attrs={props}
        min={params[local_propStart] ?? min}
        max={max}
        precision={precision}
        size={inputNumberSize}
        controlsPosition={_controlsPosition}
        placeholder={t(placeholderEnd)}
        style={localStyle}
        value={params[local_propEnd]}
        onChange={this.onChangeEnd}
        onBlur={(event) => this.$emit('blur', event, local_propEnd)}
      />
      {$slots.append || _append}
    </div>
  },
  data () {
    return {}
  },
  computed: {
    local_propStart () {
      return this.propStart || `${this.prop}Start`
    },
    local_propEnd () {
      return this.propEnd || `${this.prop}End`
    },
    localStyle () {
      let _styles = this.itemStyle
      if (this.itemWidth) _styles += `;width: ${this.itemWidth};`
      return _styles
    },
    /* has_prepend () {
      // if (this.prepend || this.$scopedSlots.prepend) return true
      if ((this.prepend || this.$scopedSlots.prepend) && this.prependVisible !== false) return true
      return false
    },
    has_append () {
      // if (this.append || this.$scopedSlots.append) return true
      if ((this.append || this.$scopedSlots.append) && this.appendVisible !== false) return true
      return false
    }, */
    inputNumberSize() {
      return this.$attrs.size || (this.$ELEMENT || {}).size
    }
  },
  methods: {
    getValidValue (value, isStart) {
      const { params, local_propStart, local_propEnd } = this
      if (isStart) {
        // 校验 start （start > end 改为 end）
        const endV = params[local_propEnd]
        if (isNaN(endV) || endV === null) return value
        if (value > endV) return endV
        return value
      } else {
        // 校验 end （end < start 改为 start）
        const startV = params[local_propStart]
        if (isNaN(startV) || value === null) return value
        if (value < startV) return startV
        return value
      }
    },
    onChangeStart (value, oldValue) {
      const { params, local_propStart } = this
      // value = this.getValidValue(value, true)
      this.$set(params, local_propStart, value)
      this.$emit('change', value, local_propStart)
    },
    onChangeEnd (value, oldValue) {
      const { params, local_propEnd } = this
      // value = this.getValidValue(value, false)
      this.$set(params, local_propEnd, value)
      this.$emit('change', value, local_propEnd)
    }
  }
}
</script>

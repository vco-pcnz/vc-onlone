<template>
  <div class="vco-number"
    ref="numberRef"
    :class="{'bold': bold, 'end': end, 'line': line}"
  >
    <a-statistic
      :prefix="showPrefix"
      :suffix="suffix"
      :precision="precision"
      :value="showValue"
    />
  </div>
</template>

<script setup>
  import { onMounted, ref, computed } from 'vue'

  const props = defineProps({
    value: {
      type: [Number, String],
      default: 0
    },
    color: {
      type: String,
      default: '#181818'
    },
    prefix: {
      type: String,
      default: '$'
    },
    suffix: {
      type: String,
      default: ''
    },
    precision: {
      type: Number,
      default: 0
    },
    bold: {
      type: Boolean,
      default: false
    },
    end: {
      type: Boolean,
      default: false
    },
    line: {
      type: Boolean,
      default: false
    }
  })

  const numberRef = ref()
  const showPrefix = computed(() => {
    let res = ''
    if (props.prefix) {
      res = Number(props.value) < 0 ? '-$' : '$'
    }
    return res
  })
  const showValue = computed(() => {
    return Math.abs(Number(props.value))
  })

  const setNumberColor = () => {
    const content = numberRef.value.querySelectorAll('.ant-statistic-content')[0]
    content.style.color = props.color
  }

  onMounted(() => {
    setNumberColor()
  })
</script>

<style lang="less" scoped>
.vco-number {
  &.line {
    :deep(.ant-statistic-content) {
      .ant-statistic-content-prefix,
      .ant-statistic-content-value-decimal,
      .ant-statistic-content-suffix,
      .ant-statistic-content-value-int {
        text-decoration: line-through;
      }
    }
  }
  &.bold {
    :deep(.ant-statistic-content) {
      font-weight: 500;
    }
  }
  &.end {
    :deep(.ant-statistic-content-value-decimal) {
      opacity: 0.5;
    }
  }
  :deep(.ant-statistic-content) {
    font-family: Aeonik, Arial, Helvetica, sans-serif;
    .ant-statistic-content-prefix {
      margin-inline-end: 0;
    }
    .ant-statistic-content-suffix {
      margin-inline-start: 0;
    }
  }
}
</style>
<template>
  <div class="capsule-chart">
    <template v-if="mergedConfig">
      <div class="label-column">
        <div v-for="item in mergedConfig.data" :key="item.name">{{ item.name }}</div>
        <div>&nbsp;</div>
      </div>

      <div class="capsule-container">
        <div
          class="capsule-item"
          v-for="(capsule, index) in capsuleLength"
          :key="index"
        >
          <div :style="`width: ${capsule * 100}%; background-color: ${mergedConfig.colors[index % mergedConfig.colors.length]};`"></div>
        </div>

        <div class="unit-label">
          <div v-for="(label, index) in labelData" :key="label + index">{{ label }}</div>
        </div>
      </div>

      <div class="unit-text" v-if="mergedConfig.unit">{{ mergedConfig.unit }}</div>
    </template>
  </div>
</template>

<script>
import { deepMerge } from '@jiaminghi/charts/lib/util/index'

import { deepClone } from '@jiaminghi/c-render/lib/plugin/util'

export default {
  name: 'DvCapsuleChart',
  props: {
    config: {
      type: Object,
      default: () => ({})
    }
  },
  data () {
    return {
      defaultConfig: {
        /**
         * @description Capsule chart data
         * @type {Array<Object>}
         * @default data = []
         * @example data = [{ name: 'foo1', value: 100 }, { name: 'foo2', value: 100 }]
         */
        data: [],
        /**
         * @description Colors (hex|rgb|rgba|color keywords)
         * @type {Array<String>}
         * @default color = ['#37a2da', '#32c5e9', '#67e0e3', '#9fe6b8', '#ffdb5c', '#ff9f7f', '#fb7293']
         * @example color = ['#000', 'rgb(0, 0, 0)', 'rgba(0, 0, 0, 1)', 'red']
         */
        colors: ['#37a2da', '#32c5e9', '#67e0e3', '#9fe6b8', '#ffdb5c', '#ff9f7f', '#fb7293'],
        /**
         * @description Chart unit
         * @type {String}
         * @default unit = ''
         */
        unit: ''
      },

      mergedConfig: null,

      capsuleLength: [],
      labelData: []
    }
  },
  watch: {
    config () {
      const { calcData } = this

      calcData()
    }
  },
  methods: {
    calcData () {
      const { mergeConfig, calcCapsuleLengthAndLabelData } = this

      mergeConfig()

      calcCapsuleLengthAndLabelData()
    },
    mergeConfig () {
      let { config, defaultConfig } = this

      this.mergedConfig = deepMerge(deepClone(defaultConfig, true), config || {})
    },
    calcCapsuleLengthAndLabelData () {
      const { data } = this.mergedConfig

      if (!data.length) return

      const capsuleValue = data.map(({ value }) => value)

      const maxValue = Math.max(...capsuleValue)

      this.capsuleLength = capsuleValue.map(v => maxValue ? v / maxValue : 0)

      const oneFifth = maxValue / 5

      this.labelData = new Array(6).fill(0).map((v, i) => Math.ceil(i * oneFifth))
    }
  },
  mounted () {
    const { calcData } = this

    calcData()
  }
}
</script>

<style lang="less">
.capsule-chart {
  position: relative;
  display: flex;
  flex-direction: row;
  box-sizing: border-box;
  padding: 10px;
  color: #fff;

  .label-column {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    box-sizing: border-box;
    padding-right: 10px;
    text-align: right;
    font-size: 12px;

    div {
      height: 20px;
      line-height: 20px;
    }
  }

  .capsule-container {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  .capsule-item {
    box-shadow: 0 0 3px #999;
    height: 10px;
    margin: 5px 0px;
    border-radius: 5px;

    div {
      height: 8px;
      margin-top: 1px;
      border-radius: 5px;
      transition: all 0.3s;
    }
  }

  .unit-label {
    height: 20px;
    font-size: 12px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }

  .unit-text {
    text-align: right;
    display: flex;
    align-items: flex-end;
    font-size: 12px;
    line-height: 20px;
    margin-left: 10px;
  }
}
</style>

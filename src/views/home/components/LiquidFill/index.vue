<template>
  <ve-liquidfill
    :data="chartData"
    :settings="chartSettings"
    height="100%"
  />
</template>

<script type="text/ecmascript-6">
import CommonDataMixin from '@/mixins/commonDataMixin'

function getColor(value) {
  console.log('getColor -> value', value)
  return value > 0 && value <= 0.5 ? 'rgba(97, 216, 0, .7)' : (
    value > 0.5 && value <= 0.8 ? 'rgba(204,178,26,.7)' : (
      value > 0.8 ? 'rgba(241,47,28,.7)' : '#c7c7cb'
    )
  )
}

export default {
  name: '',

  mixins: [CommonDataMixin],

  data() {
    return {
      chartData: {},
      chartSettings: {}
    }
  },

  watch: {
    userGrowthLastMonth() {
      console.log(this.userGrowthLastMonth)
      this.chartData = {
        columns: ['title', 'percent'], // 第一个元素时label, 第二个元素时value
        rows: [{
          title: '用户月同比增长',
          percent: this.userGrowthLastMonth / 100
        }]
      }
      this.chartSettings = {
        seriesMap: {
          用户月同比增长: {
            radius: '80%',
            label: {
              formatter: v => {
                return `${(v.data.value * 100).toFixed(2)}%`
              },
              textStyle: {
                fontSize: 36,
                color: '#999',
                fontWeight: 'normal'
              },
              position: ['50%', '50%'],
              insideColor: '#fff' // 波浪和label重叠时，label的颜色
            },
            outline: {
              borderDistance: 0,
              itemStyle: {
                borderColor: '#aaa4a4',
                borderWidth: 1,
                color: 'none',
                shadowBlur: 0,
                shadowColor: '#fff'
              }
            },
            backgroundStyle: {
              color: '#fff' // 背景色
            },
            itemStyle: {
              shadowBlur: 0, // 去掉背景默认阴影
              shadowColor: '#fff'
            },
            amplitude: 8,
            color: [getColor(this.chartData.rows[0].percent)]
          }
        }
      }
    }
  },

  mounted() {

  },

  methods: {

  }
}
</script>

<style lang="scss" scoped>
</style>

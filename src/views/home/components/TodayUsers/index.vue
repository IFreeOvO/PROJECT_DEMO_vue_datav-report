<template>
  <div>
    <common-card
      title="今日交易用户数"
      :value="orderUser"
    >
      <template>
        <v-chart :options="getOptions()" />
      </template>
      <template v-slot:footer>
        <div>
          <span>退货率</span>
          <span class="emphasis">{{ returnRate }}</span>
        </div>
      </template>
    </common-card>
  </div>
</template>
<script type="text/ecmascript-6">
import CommonCardMixin from '@/mixins/commonCardMixin'
import CommonDataMixin from '@/mixins/commonDataMixin'

export default {
  name: 'TodayUsers',

  mixins: [CommonCardMixin, CommonDataMixin],

  data() {
    return {

    }
  },

  mounted() {
  },

  methods: {
    getOptions() {
      return {
        color: ['#3398DB'],
        tooltip: {},
        xAxis: {
          type: 'category',
          show: false,
          data: this.orderUserTrendAxis
        },
        yAxis: {
          show: false
        },
        series: [{
          name: '用户实时交易量',
          type: 'bar',
          barWidth: '60%',
          data: this.orderUserTrend
        }],
        grid: {
          top: 0,
          left: 0,
          bottom: 0,
          right: 0
        }
      }
    }
  }
}
</script>
<style lang="scss" scoped>
</style>

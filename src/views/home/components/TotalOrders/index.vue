<template>
  <div>
    <common-card
      title="累计订单量"
      :value="orderToday"
    >
      <template>
        <v-chart :options="getOptions()" />
      </template>
      <template v-slot:footer>
        <div>
          <span>昨日订单量</span>
          <span class="emphasis">{{ orderLastDay }}</span>
        </div>
      </template>
    </common-card>
  </div>
</template>
<script type="text/ecmascript-6">
import CommonCardMixin from '@/mixins/commonCardMixin'
import CommonDataMixin from '@/mixins/commonDataMixin'

export default {
  name: 'TotalOrders',

  mixins: [CommonCardMixin, CommonDataMixin],

  data() {
    return {

    }
  },

  mounted() {
  },

  methods: {
    getOptions() {
      return this.orderTrend.length > 0 ? (
        {
          xAxis: {
            type: 'category',
            show: false,
            boundaryGap: false // 消除x轴两侧间距
          },
          yAxis: {
            show: false
          },
          series: [{
            type: 'line',
            data: this.orderTrend,
            areaStyle: {
              color: 'purple'
            },
            lineStyle: {
              width: 0
            },
            itemStyle: {
              opacity: 0
            },
            smooth: true
          }],
          grid: {
            top: 0,
            left: 0,
            bottom: 0,
            right: 0
          }
        }
      ) : {}
    }
  }
}
</script>
<style lang="scss" scoped>
</style>

<template>
  <div>
    <common-card
      title="累计用户数"
      :value="userToday"
    >
      <template>
        <v-chart :options="getOptions()" />
      </template>
      <template v-slot:footer>
        <div class="total-users-footer">
          <span>日同比</span>
          <span class="emphasis">{{ userGrowthLastDay }}</span>
          <div class="increase" />
          <span class="month">月同比</span>
          <span class="emphasis">{{ userGrowthLastMonth }}</span>
          <div class="decrease" />
        </div>
      </template>
    </common-card>
  </div>
</template>
<script type="text/ecmascript-6">
import CommonCardMixin from '@/mixins/commonCardMixin'
import CommonDataMixin from '@/mixins/commonDataMixin'

export default {
  name: 'TotalUsers',

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
        xAxis: {
          type: 'value',
          show: false
        },
        yAxis: {
          type: 'category',
          show: false
        },
        series: [{
          name: '上月平台用户数',
          type: 'bar',
          stack: '总量', // 可以让相同stack的图重合
          data: [this.userLastMonth],
          barWidth: 10,
          itemStyle: {
            color: '#45c946'
          }
        },
        {
          name: '今日平台用户数',
          type: 'bar',
          stack: '总量',
          data: [this.userTodayNumber],
          barWidth: 10,
          itemStyle: {
            color: '#eee'
          }
        },
        {
          type: 'custom', // 自定义图形
          data: [this.userLastMonth],
          stack: '总量',
          renderItem: (params, api) => {
            // console.log('mounted -> params, api', params, api)
            const value = api.value(0) // 拿到data数组里第一个数据
            const endPoint = api.coord([value, 0]) // 数值转换成坐标
            // 返回要绘制的图形
            return {
              type: 'group',
              position: endPoint,
              children: [
                {
                  type: 'path',
                  shape: {
                    d: 'M1024 255.996 511.971 767.909 0 255.996 1024 255.996z', // 对应的svg图标
                    x: -5,
                    y: -20,
                    width: 10,
                    height: 10,
                    layout: 'cover' // 缩放图形
                  },
                  style: {
                    fill: '#45c946'
                  }
                },
                {
                  type: 'path',
                  shape: {
                    d: 'M0 767.909l512.029-511.913L1024 767.909 0 767.909z', // 对应的svg图标
                    x: -5,
                    y: 10,
                    width: 10,
                    height: 10,
                    layout: 'cover'
                  },
                  style: {
                    fill: '#45c946'
                  }
                }
              ]
            }
          }
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
.total-users-footer {
  display: flex;
  align-items: center;
  .month {
    margin-left: 10px;
  }
}
</style>

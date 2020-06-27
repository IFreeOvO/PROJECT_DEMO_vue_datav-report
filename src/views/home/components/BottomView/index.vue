<template>
  <div class="bottom-view">
    <div class="view">
      <el-card shadow="hover">
        <template v-slot:header>
          <div class="title-wrapper">关键词搜索</div>
        </template>
        <template>
          <div class="chart-wrapper">
            <div class="chart-inner">
              <div class="chart">
                <div class="chart-title">搜索用户数</div>
                <div class="chart-data">{{ userCount | format }}</div>
                <v-chart :options="searchUserOption" />
              </div>
              <div class="chart">
                <div class="chart-title">搜索量</div>
                <div class="chart-data">{{ searchCount | format }}</div>
                <v-chart :options="searchNumberOption" />
              </div>
            </div>
            <div class="table-wrapper">
              <el-table :data="tableData">
                <el-table-column
                  prop="rank"
                  label="排名"
                  width="100"
                />
                <el-table-column
                  prop="keyword"
                  label="关键字"
                  width="100"
                />
                <el-table-column
                  prop="count"
                  label="总搜索量"
                />
                <el-table-column
                  prop="users"
                  label="搜索用户数"
                />
                <el-table-column
                  prop="range"
                  label="搜索占比"
                />
              </el-table>
              <el-pagination
                layout="prev, pager, next"
                :total="total"
                :page-size="pageSize"
                background
                @current-change="onPageChange"
              />
            </div>
          </div>
        </template>
      </el-card>
    </div>
    <div class="view">
      <el-card shadow="hover">
        <template v-slot:header>
          <div class="title-wrapper">
            <div class="title">分类销售排行</div>
            <div class="radio-wrapper">
              <el-radio-group
                v-model="radioSelect"
                size="small"
                @change="onPieChartCategory"
              >
                <el-radio-button label="品类" />
                <el-radio-button label="商品" />
              </el-radio-group>
            </div>
          </div>
        </template>
        <template>
          <div class="chart-wrapper">
            <v-chart :options="categoryOptions" />
          </div>
        </template>
      </el-card>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import CommonDataMixin from '@/mixins/commonDataMixin'

const colors = ['#8d7fec', '#5085f2', '#f8726b', '#e7e702', '#78f283', '#4bc1fc']

export default {
  name: 'BottomView',

  mixins: [CommonDataMixin],

  data() {
    return {
      searchUserOption: {},
      searchNumberOption: {},
      tableData: [],
      // tableData: [
      //   {
      //     id: 1,
      //     rank: 1,
      //     keyword: '背景',
      //     count: 1231,
      //     users: 210,
      //     range: '90%'
      //   },
      //   {
      //     id: 2,
      //     rank: 1,
      //     keyword: '背景',
      //     count: 1231,
      //     users: 210,
      //     range: '90%'
      //   },
      //   {
      //     id: 3,
      //     rank: 1,
      //     keyword: '背景',
      //     count: 1231,
      //     users: 210,
      //     range: '90%'
      //   },
      //   {
      //     id: 4,
      //     rank: 1,
      //     keyword: '背景',
      //     count: 1231,
      //     users: 210,
      //     range: '90%'
      //   }
      // ],
      radioSelect: '品类',
      categoryOptions: {},
      totalData: [],
      total: 0,
      pageSize: 4,
      userCount: 0,
      searchCount: 0
    }
  },

  watch: {
    wordCloud(newValue) {
      const totalData = []
      newValue.forEach((item, index) => {
        totalData.push({
          id: index + 1,
          rank: index + 1,
          keyword: item.word,
          count: item.count,
          users: item.user,
          range: `${((item.user / item.count) * 100).toFixed(2)}%`
        })
      })
      this.totalData = totalData
      this.total = this.totalData.length
      this.renderPage(1)
      // console.log('wordCloud -> this.totalData', this.totalData)

      this.userCount = totalData.reduce((s, i) => i.users + s, 0)
      this.searchCount = totalData.reduce((s, i) => i.count + s, 0)
      this.renderLineChart()
    },

    category1(newValue) {
      this.renderPieChart()
    }
  },

  mounted() {
    // this.renderPieChart()
  },

  methods: {
    onPieChartCategory(type) {
      this.radioSelect = type
      this.renderPieChart()
    },

    onPageChange(page) {
      // console.log('onPageChange -> page', page)
      this.renderPage(page)
    },

    renderPieChart() {
      if (!this.category1.data1 || !this.category2.data1) {
        return
      }

      let data
      let axis
      let total = 0

      if (this.radioSelect === '品类') {
        data = this.category1.data1.slice(0, 6)
        axis = this.category1.axisX.slice(0, 6)
        total = data.reduce((s, i) => s + i, 0)
      } else {
        data = this.category2.data1.slice(0, 6)
        axis = this.category2.axisX.slice(0, 6)
        total = data.reduce((s, i) => s + i, 0)
      }
      const chartData = []
      data.forEach((item, index) => {
        const percent = `${(item / total * 100).toFixed(2)}%`
        chartData.push({
          legendname: axis[index],
          value: item,
          percent,
          itemStyle: {
            color: colors[index]
          },
          name: `${axis[index]} | ${percent}`
        })
      })

      // const mockData = [
      //   {
      //     legendname: '面粉粥店',
      //     value: 67,
      //     percent: '15.40%',
      //     itemStyle: {
      //       color: '#e7e702'
      //     },
      //     name: '面粉粥店 | 15.40%'
      //   },
      //   {
      //     legendname: '简餐便当',
      //     value: 97,
      //     percent: '22.30%',
      //     itemStyle: {
      //       color: '#8d7fec'
      //     },
      //     name: '简餐便当 | 15.40%'
      //   },
      //   {
      //     legendname: '汉堡披萨',
      //     value: 92,
      //     percent: '21.15%',
      //     itemStyle: {
      //       color: '#5085f2'
      //     },
      //     name: '汉堡披萨 | 15.40%'
      //   }
      // ]

      this.categoryOptions = {
        title: [
          {
            text: `${this.radioSelect}分布`,
            textStyle: {
              fontSize: 14,
              color: '#666'
            },
            left: 20,
            top: 20
          },
          {
            text: '累计订单量',
            subtext: total, // 副标题
            textStyle: {
              fontSize: 14,
              color: '#999'
            },
            subtextStyle: {
              fontSize: 28,
              color: '#333'
            },
            x: '34.5%',
            y: '42.5%',
            textAlign: 'center'
          }
        ],
        series: [
          {
            name: '标题', // 用于区分不同图形
            type: 'pie',
            data: chartData,
            label: {
              normal: {
                show: true,
                position: 'outter',
                // 饼图外围一圈的标签
                formatter: function(params) {
                  // console.log('renderPieChart -> params', params)
                  return params.data.legendname
                }
              }
            },
            center: ['35%', '50%'], // 圆心坐标
            radius: ['45%', '60%'], // 内半径和外半径
            labelLine: {
              normal: {
                length: 5, // label的线段长度
                length2: 3,
                smooth: true
              }
            },
            clockwise: false, // 数据顺时针排序
            itemStyle: {
              borderWidth: 4,
              borderColor: '#fff'
            }
          }
        ],
        legend: {
          type: 'scroll', // 内容超过height时可以滚动
          orient: 'vertical',
          height: 250,
          left: '70%',
          top: 'middle',
          textStyle: {
            color: '#8c8c8c'
          }
        },
        tooltip: {
          trigger: 'item',
          formatter: function(params) {
            // console.log('renderPieChart -> params', params)
            const str = params.seriesName + '<br/>' +
              params.marker + params.data.legendname + '<br/>' +
              '数量: ' + params.data.value + '<br/>' +
              '占比: ' + params.data.percent
            return str
          }
        }
      }
    },

    renderPage(page) {
      this.tableData = this.totalData.slice((page - 1) * this.pageSize, (page - 1) * this.pageSize + this.pageSize)
    },

    renderLineChart() {
      const createOption = key => {
        const data = []
        const axis = []
        this.wordCloud.forEach(item => data.push(item[key]))
        this.wordCloud.forEach(item => axis.push(item.word))

        return {
          xAxis: {
            type: 'category',
            // show: false,
            boundaryGap: false, // 消除x轴两侧间距
            data: axis
          },
          yAxis: {
            show: false
            // min: 0,
            // max: 300 // 坐标系范围，防止图形顶边被遮住
          },
          tooltip: {},
          series: [{
            type: 'line',
            data,
            areaStyle: {
              color: 'rgba(95,187,255,.5)'
            },
            lineStyle: {
              color: 'rgba(95,187,255)'
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
      }

      this.searchUserOption = createOption('user')
      this.searchNumberOption = createOption('count')
    }
  }
}
</script>
<style lang="scss" scoped>
.bottom-view {
  display: flex;
  margin-top: 20px;
  .view {
    flex: 1;
    width: 50%;
    box-sizing: border-box;
    &:first-child {
      padding: 0 10px 0 0;
    }
    &:last-child {
      padding: 0 0 0 10px;
    }
    .title-wrapper {
      display: flex;
      align-items: center;
      height: 60px;
      box-sizing: border-box;
      border-bottom: 1px solid #eee;
      font-size: 14px;
      font-weight: 500;
      padding: 0 0 0 20px;
      .radio-wrapper {
        flex: 1;
        display: flex;
        justify-content: flex-end;
        padding-right: 20px;
      }
    }
    .chart-wrapper {
      display: flex;
      flex-direction: column;
      height: 452px;
      .chart-inner {
        display: flex;
        padding: 0 10px;
        margin-top: 20px;
        .chart {
          flex: 1;
          padding: 0 10px;
          .chart-title {
            color: #999;
            font-size: 14px;
          }
          .chart-data {
            font-size: 22px;
            color: #333;
            font-weight: 500;
            letter-spacing: 2px;
          }
          .echarts {
            height: 50px;
          }
        }
      }
      .table-wrapper {
        flex: 1;
        margin-top: 20px;
        padding: 0 20px 20px;
        .el-pagination {
          display: flex;
          justify-content: flex-end;
          margin-top: 15px;
        }
      }
    }
  }
}
</style>

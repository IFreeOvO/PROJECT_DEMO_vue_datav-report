<template>
  <common-card
    title="累计销售额"
    :value="salesToday"
  >
    <template>
      <div class="compare-wrapper">
        <div class="compare">
          <span>日同比</span>
          <span class="emphasis">{{ salesGrowthLastDay }}</span>
          <div class="increase" />
        </div>
        <div class="compare">
          <span>月同比</span>
          <span class="emphasis">{{ salesGrowthLastMonth }}</span>
          <div class="decrease" />
        </div>
      </div>
    </template>
    <template v-slot:footer>
      <div>
        <span>昨日销售额</span>
        <span class="emphasis">{{ salesLastDay }}</span>
      </div>
    </template>
  </common-card>
</template>

<script type="text/ecmascript-6">
import CommonCardMixin from '@/mixins/commonCardMixin'
import CommonDataMixin from '@/mixins/commonDataMixin'

export default {
  name: 'TotalSales',

  mixins: [CommonCardMixin, CommonDataMixin],

  data() {
    return {

    }
  },

  methods: {

  }
}
</script>

<style lang="scss" scoped>
.compare-wrapper {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  .compare {
    display: flex;
    align-items: center;
    height: 50%;
    font-size: 12px;
    margin-top: 3px;
    color: #666;
  }
}

</style>

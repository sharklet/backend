<style lang="less"></style>
<template>
  <div class="h-panel w-1200">
    <div class="h-panel-bar">
      <span class="h-panel-title">每日订单量统计</span>
    </div>
    <div class="h-panel-body">
      <div class="float-box mb-10">
        <Form :labelWidth="110">
          <Row :space="10">
            <Cell :width="6">
              <FormItem label="时间范围">
                <DateRangePicker v-model="daterange"></DateRangePicker>
              </FormItem>
            </Cell>
            <Cell :width="6">
              <FormItem>
                <Button color="primary" @click="getData(true)">过滤</Button>
              </FormItem>
            </Cell>
          </Row>
        </Form>
      </div>
      <div class="float-box mb-10">
        <line-chart :height="200" :chart-data="data"></line-chart>
      </div>
    </div>
  </div>
</template>
<script>
import LineChart from 'components/common/chartjs/line';

export default {
  components: {
    LineChart
  },
  props: ['id'],
  data() {
    return {
      data: {},
      daterange: {
        start: null,
        end: null
      }
    };
  },
  mounted() {
    this.getData();
  },
  methods: {
    getData() {
      let data = {};
      if (this.daterange.start) {
        data.start_at = this.daterange.start;
      }
      if (this.daterange.end) {
        data.end_at = this.daterange.end;
      }
      R.Statistic.orderCreated(data).then(resp => {
        let data = {
          labels: resp.data.labels,
          datasets: [
            {
              label: '订单数',
              data: resp.data.dataset
            }
          ]
        };
        this.data = data;
      });
    }
  }
};
</script>

<template>
  <div class="page page-category" v-if="category.visible && !category.metrics.every(m => !m.visible)">
    <div class="row">
      <div class="col-xs-6 text-left">
      <h1>{{ category.name }}</h1></div>
    </div>
    <div class="row">
      <div class="col-xs-12">
        <ReportMetric v-for="m in category.metrics"
                      :key="m.id"
                      :metric="m"
                      :metric-values="metricValues.hasOwnProperty(m.metric) ? metricValues[m.metric] : null"
                      :county-averages="countyAverages.hasOwnProperty(m.metric) ? countyAverages[m.metric] : null"
                      :visible="m.visible"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ReportMetric from './report-metric';

export default {
  name: 'ReportCategory',
  components: {
    ReportMetric,
  },
  props: {
    category: {
      type: Object,
      required: true,
    },
    metricValues: {
      type: Object,
      required: false,
      default: () => {},
    },
    countyAverages: {
      type: Object,
      required: false,
      default: () => {},
    },
  },
};
</script>

<style scoped>
.page-category h1 {
    margin: 0;
    text-shadow: 1px 1px 1px #ccc;
}
.page-category .table {
    margin-top: 30px;
}
.page-category .table td {
    white-space: nowrap;
    font-size: 0.9em;
}
.page-category .table td:first-of-type {
    white-space: normal;
}
.page-category .report-column-selected {
    background: #dce6f0;
}
.page-category .report-column-county {
    background: #ecf0df;
}
</style>

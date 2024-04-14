<script>
import {mapState} from "vuex";
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

export default {
  name: "AnalyticsPage",
  mounted() {
    if (localStorage.getItem("leadhit-site-id") === null) {
      this.$router.push('/auth');
    }

    let chart = am4core.create(this.$refs.chartdiv, am4charts.XYChart);
    chart.paddingRight = 20;
    chart.data = this.visits;

    let dateAxis = chart.xAxes.push(new am4charts.DateAxis());
    dateAxis.renderer.grid.template.location = 0;

    let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis.tooltip.disabled = true;
    valueAxis.renderer.minWidth = 35;

    let series = chart.series.push(new am4charts.LineSeries());
    series.dataFields.dateX = "date";
    series.dataFields.valueY = "visits";

    series.tooltipText = "{valueY.value}";
    chart.cursor = new am4charts.XYCursor();

    this.chart = chart;
  },
  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  },
  computed: {
    ...mapState(['visits']),
  }
}
</script>

<template>
  <div class="analytics">
    <h1>Аналитика</h1>
    <h2>Аналитика по визитам</h2>
    <div class="chart-wrapper" ref="chartdiv">
    </div>
  </div>
</template>

<style scoped>
.chart-wrapper {
  width: 100%;
  height: 500px;
}
</style>
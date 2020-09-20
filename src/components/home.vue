<template>
  <div>
    <ve-scatter :data="chartData" :settings="chartSettings" :extend="extend" />
  </div>
</template>

<script>
// 按需引入散点图
import { VeScatter } from "v-charts";
import traceList from "@/assets/json/trace-array.json";
export default {
  name: "home",
  components: { VeScatter },
  data() {
    return {
      chartSettings: {
        labelMap: {
          // startTime
        },
        // x轴数据类型，按时间大小排序
        xAxisType: "time",
        xAxisName: "Time",
        yAxisName: "Duration",
      },
      extend: {
        // 标题
        title: {
          text: "Trace Datas",
        },
        tooltip: {
          // 鼠标移入时数据显示格式化
          formatter: (params) => {
            return "frontend：" +  params[1].data[1];
          },
        },
        legend: {
          data: []
        },
        xAxis: {
          axisLabel: {
            // x轴数据格式化
            formatter: (v) => {
              const date = new Date(~~v);
              const h24 = date.getHours();
              const h = (h24 % 12).toString().padStart(2, 0);
              const m = date.getMinutes().toString().padStart(2, 0);
              const s = date.getSeconds().toString().padStart(2, 0);
              return h + ":" + m + ":" + s + " " + (h24 >= 12 ? "pm" : "am");
            },
          },
        },
        yAxis: {
          axisLabel: {
            // y轴数据格式化
            formatter: "{value}ms",
          },
        },
      },
      chartData: {
        columns: ["startTime", "duration", "operationName"],
        rows: traceList.data[0].spans,
      },
    };
  },
};
</script>

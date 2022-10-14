<template>
  <BarChart class="max-w-1/2" ref="barRef" :chartData="testData" :options="options" />
</template>

<script>
import { Chart, registerables } from "chart.js";
import { defineComponent, ref } from "vue";
import { BarChart } from "vue-chart-3";

Chart.register(...registerables);

export default defineComponent({
  name: "Home",
  components: { BarChart },
  setup() {
    const barRef = ref();

    const options = ref({
      barThickness: 80,
      responsive: true,
      indexAxis: "x",
      plugins: {
        tooltip: {
          position: "middle",
        },
      },
    });

    const testData = {
      labels: ["El salvador", "Nîmes", "Toulon", "Perpignan", "Autre"],
      options: {
        indexAxis: "x",
      },
      datasets: [
        {
          data: [96, 40, 60, 70, 5],
          backgroundColor: [
            "#000",
            "#0079AF",
            "#123E9B",
            "#97B0C4",
            "#A5C8ED",
            "#A5C8ED",
          ],
        },
      ],
    };

    return { barRef, options, testData };
  },
});
</script>
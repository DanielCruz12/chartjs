<template>
  <BarChart ref="barRef" :chartData="testData" :options="options" />
</template>

<script>
import { Chart, registerables } from "chart.js";
import { defineComponent, ref } from "vue";
import { BarChart } from "vue-chart-3";

Chart.register(...registerables);

// const tooltipPlugin = Chart.registry.getPlugin("tooltip");
// tooltipPlugin.positioners.middle = function (elements, eventPosition) {
//   console.log("elements", elements);
//   console.log("eventPosition", eventPosition);

//   // const el = items[0].element;
//   // let xPos = 0;
//   // let yPos = 0;

//   // if (el && el.hasValue()) {
//   //   const { base, horizontal, x, y } = el.getProps();
//   //   if (horizontal) {
//   //     xPos = (base + x) / 2;
//   //     yPos = y;
//   //   } else {
//   //     xPos = x;
//   //     yPos = (base + y) / 2;
//   //   }
//   // }

//   // return {
//   //   x: xPos,
//   //   y: yPos,
//   // };
//   return true;
// };

export default defineComponent({
  name: "Home",
  components: { BarChart },
  setup() {
    const barRef = ref();

    const options = ref({
      barThickness: 20,
      responsive: true,
      indexAxis: "y",
      plugins: {
        tooltip: {
          // position: "middle",
        },
      },
    });

    const testData = {
      labels: ["Paris", "Nîmes", "Toulon", "Perpignan", "Autre"],
      options: {
        barThickness: 20,
        indexAxis: "y",
      },
      datasets: [
        {
          data: [30, 40, 60, 70, 5],
          backgroundColor: [
            "#77CEFF",
            "#0079AF",
            "#123E6B",
            "#97B0C4",
            "#A5C8ED",
          ],
        },
      ],
    };

    return { barRef, options, testData };
  },
});
</script>
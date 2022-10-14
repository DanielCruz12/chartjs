<template>
  <div>
    <div class="mb-6">
      <div class="mb-6">
        <canvas ref="canvasRef" height="200" width="200" />
      </div>

      <div class="border-b border-charcoal-100" />
    </div>
  </div>
</template>

<script>
import { onMounted, ref } from "vue";
import { Chart, registerables } from "chart.js";

export default {
  name: "HorizontalGroupedBarChart",
  props: {
    payload: {
      type: Object,
      default: null,
    },
  },
  setup(props) {
    const canvasRef = ref(null);

    Chart.register(...registerables);

    onMounted(() => {
      const ctx = canvasRef.value.getContext("2d");

      new Chart(ctx, {
        type: "bar",
        data: {
          labels: ["1AM", "2AM", "3AM", "4AM", "5AM", "6AM"],
          datasets: [
            {
              label: "Awake",
              data: [1, 2, 0, 0, 0, 0],
              backgroundColor: "#654980",
              barThickness: 20,
              borderSkipped: "false",
              borderRadius: 9999,
            },
            {
              label: "Asleep",
              data: [0, 0, 3, 4, 0, 0],
              backgroundColor: "#E0DBE6",
              barThickness: 20,
              borderSkipped: "false",
              borderRadius: 9999,
            },
            {
              label: "Disrupted",
              data: [0, 0, 0, 0, 5, 6],
              backgroundColor: "yellow",
              barThickness: 20,
              borderSkipped: "false",
              borderRadius: 9999,
            },
          ],
        },
        options: {
          indexAxis: "x",
          scales: {
            x: {
              stacked: true,
            },
            y: {
              stacked: true,
            },
          },
        },
      });
    });

    return { canvasRef };
  },
};
</script>

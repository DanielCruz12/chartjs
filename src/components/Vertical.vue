<template>
  <div>
    <div class="mb-6">
      <div class="mb-6">
        <canvas ref="canvasRef" height="100" width="200" />
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

      // new Chart(ctx, {
      //   type: 'bar',
      //   data: {
      //     labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
      //     datasets: [
      //       {
      //         label: '# of Votes',
      //         data: [12, 19, 3, 5, 2, 3],
      //         backgroundColor: [
      //           'rgba(255, 99, 132, 0.2)',
      //           'rgba(54, 162, 235, 0.2)',
      //           'rgba(255, 206, 86, 0.2)',
      //           'rgba(75, 192, 192, 0.2)',
      //           'rgba(153, 102, 255, 0.2)',
      //           'rgba(255, 159, 64, 0.2)',
      //         ],
      //         borderColor: [
      //           'rgba(255, 99, 132, 1)',
      //           'rgba(54, 162, 235, 1)',
      //           'rgba(255, 206, 86, 1)',
      //           'rgba(75, 192, 192, 1)',
      //           'rgba(153, 102, 255, 1)',
      //           'rgba(255, 159, 64, 1)',
      //         ],
      //         borderWidth: 1,
      //       },
      //     ],
      //   },
      //   options: {
      //     scales: {
      //       y: {
      //         beginAtZero: true,
      //       },
      //     },
      //   },
      // });

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

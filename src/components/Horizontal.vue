<template>
  <div class="-my-8">
    <div class="flex items-center">
      <div v-if="label" class="text-center w-1/12">
        <p class="-ml-2 text-copy-small" v-text="label" />
      </div>

      <div :class="label ? 'w-11/12' : 'w-full'">
        <canvas ref="canvasRef" />
      </div>
    </div>
  </div>

  <div
    v-if="hasFooter"
    class="flex justify-between mb-6 mt-4 text-charcoal-300"
    :class="{ 'ml-7': label }"
  >
    <p
      v-for="(item, index) in getTimes()"
      :key="index"
      class="text-label-tiny uppercase"
      v-text="item"
    />
  </div>
</template>

<script>
import { onMounted, ref } from "vue";
import { Chart, registerables } from "chart.js";

Chart.register(...registerables);

const tooltipPlugin = Chart.registry.getPlugin("tooltip");
tooltipPlugin.positioners.middle = function (items) {
  if (items.length !== 1) {
    // For more than 1 item, just show at the nearest
    return tooltipPlugin.positioners.average(items);
  }

  const el = items[0].element;
  let xPos = 0;
  let yPos = 0;

  if (el && el.hasValue()) {
    const { base, horizontal, x, y } = el.getProps();
    if (horizontal) {
      xPos = (base + x) / 2;
      yPos = y;
    } else {
      xPos = x;
      yPos = (base + y) / 2;
    }
  }

  return {
    x: xPos,
    y: yPos,
  };
};

export default {
  name: "HorizontalGroupedBarChart",
  props: {
    hasFooter: {
      type: Boolean,
      default: true,
    },
    label: {
      type: String,
      default: null,
    },
    payload: {
      type: Object,
      default: () => {},
    },
  },
  setup(props) {
    const canvasRef = ref(null);

    const getSecondsFromDuration = (time) => {
      const [h, m] = time.split(":");
      const seconds = h * 3600 + m * 60;
      return seconds;
    };

    /**
     * Gets the combined total duration of the chart.
     *
     * @returns {number} The total duration.
     */
    const getTotalDuration = () => {
      let totalDuration = 0;
      props.payload.timeline.forEach((item) => {
        totalDuration += getSecondsFromDuration(item.duration);
      });
      return totalDuration;
    };

    /**
     * Gets the datasets for the chart.
     *
     * @returns {array} The datasets array.
     */
    const getDatasets = () => {
      return props.payload.timeline.map((item, index) =>
        createDataset(item, index)
      );
    };

    /**
     * Creates an individual dataset.
     *
     * @param {string} item - The timeline item.
     * @param {number} index - The index.
     * @returns {object} The dataset object.
     */
    const createDataset = (item, index) => {
      let color = null;
      if (item.state.toLowerCase() === "awake") {
        color = "#f9f8f7"; // bone-100
      } else if (item.state.toLowerCase() === "asleep") {
        color = "#654980"; // dark-purple
      } else {
        color = "#e0dbe6"; // light-purple
      }

      let borderRadius = null;
      if (index === 0) {
        borderRadius = {
          topLeft: 9999,
          bottomLeft: 9999,
        };
      } else if (index === props.payload.timeline.length - 1) {
        borderRadius = {
          topRight: 9999,
          bottomRight: 9999,
        };
      } else {
        borderRadius = 0;
      }

      return {
        label: item.state,
        data: [getSecondsFromDuration(item.duration)],
        backgroundColor: color,
        barThickness: 20,
        borderColor: color,
        borderRadius,
        borderSkipped: false,
        borderWidth: 1,
      };
    };

    /**
     * Gets the AM/PM times.
     */
    const getTimes = () => {
      // @todo: makes time dynamic when data is ready.
      // convert startTime (20:00) to '8PM'

      return ["8PM", "11PM", "1AM", "3AM", "6AM", "8AM"];
    };

    /**
     * Creates the Chart.js chart.
     */
    const createChart = () => {
      const ctx = canvasRef.value.getContext("2d");
      canvasRef.value.height = 90;

      // Chart.Tooltip.positioners.middle = (elements, position) => {
      //   console.log(elements);
      //   console.log(position);
      //   return true;
      // };

      let delayed = null;
      new Chart(ctx, {
        type: "bar",
        data: {
          labels: [props.label ? props.label : "Label"],
          datasets: getDatasets(),
        },
        options: {
          // interaction: {
          //   axis: "xy",
          //   intersect: false,
          //   mode: "index",
          // },
          barThickness: 20,
          indexAxis: "y",
          maintainAspectRatio: false,
          responsive: true,
          animation: {
            onComplete: () => {
              delayed = true;
            },
            delay: (context) => {
              let delay = 0;
              if (
                context.type === "data" &&
                context.mode === "default" &&
                !delayed
              ) {
                delay = context.dataIndex * 15 + context.datasetIndex * 5;
              }
              return delay;
            },
          },
          scales: {
            x: {
              display: false,
              max: getTotalDuration(),
              stacked: true,
            },
            y: {
              display: false,
              stacked: true,
              grid: {
                display: false,
                drawBorder: false,
              },
            },
          },
          plugins: {
            tooltip: {
              enabled: true,
              caretPadding: 10,
              caretSize: 7,
              callbacks: {
                label: (context) => {
                  let label = ` ${context.dataset.label}`;
                  return label;
                },
                labelPointStyle: (context) => {
                  return {
                    // borderColor: "white",
                    // borderWidth: 2,
                    pointStyle: "rectRounded",
                    // strokeStyle: "white",
                    // strokeWidth: 2,
                  };
                },
                title: () => null,
              },
              backgroundColor: "currentColor",
              bodyFont: {
                family: "Okta Neue",
                padding: 20,
                size: 14,
              },
              // borderColor: "white",
              // borderWidth: 4,
              padding: 7,
              // boxBorderColor: "white",
              // boxBorderWidth: 2,
              // strokeStyle: "#FFF",
              boxHeight: 16,
              boxWidth: 16,
              displayColors: true,
              position: "middle",
              usePointStyle: true,
              xAlign: "center",
              yAlign: "bottom",
            },
            legend: {
              display: false,
            },
          },
        },
      });
    };

    onMounted(() => {
      createChart();
    });

    return {
      canvasRef,
      getTimes,
    };
  },
};
</script>

<style>
@import url("https://unpkg.com/tailwindcss@2.2.17/dist/tailwind.min.css");
</style>

<template>
  <div id="lightweight-chart"></div>
</template>

<script>
import { createChart, CrosshairMode } from "lightweight-charts";
export default {
  name: "LightweightTradingview",
  data: () => ({
    chart: null,
    candleSeries: null,
    volumeSeries: null,
  }),
  computed: {
    chartConfig() {
      return {
        width: window.innerWidth,
        height: window.innerHeight,
        layout: {
          backgroundColor: "transparent",
        },
        grid: {
          vertLines: {
            color: "#ddd",
          },
          horzLines: {
            color: "#ddd",
          },
        },
        crosshair: {
          mode: CrosshairMode.Normal,
        },
        priceScale: {
          borderColor: "#485c7b",
        },
        timeScale: {
          borderColor: "#485158",
        },
        watermark: {
          // text: "XYZ",
          fontSize: 256,
          color: "rgba(256, 256, 256, 0.1)",
          visible: true,
        },
      };
    },
    candlesConfig() {
      return {
        upColor: "#4bffb5",
        downColor: "#ff4976",
        borderDownColor: "#ff4976",
        borderUpColor: "#4bffb5",
        wickDownColor: "#838ca1",
        wickUpColor: "#838ca1",
      };
    },
    histogramConfig() {
      return {
        color: "#385263",
        lineWidth: 2,
        priceFormat: {
          type: "volume",
        },
        overlay: true,
        scaleMargins: {
          top: 0.9,
          bottom: 0,
        },
      };
    },
  },
  methods: {
    controlResponsive() {
      this.chart.applyOptions({
        width: window.innerWidth,
        height: window.innerHeight,
      });

      setTimeout(() => this.chart.timeScale().fitContent(), 0);
    },
    setNextBar() {
      let nextBar = this.setNextBar;
      if (!nextBar.date) nextBar.date = new Date(2020, 0, 0);
      if (!nextBar.bar)
        nextBar.bar = { open: 100, high: 104, low: 98, close: 103 };

      nextBar.date.setDate(nextBar.date.getDate() + 1);
      nextBar.bar.time = {
        year: nextBar.date.getFullYear(),
        month: nextBar.date.getMonth() + 1,
        day: nextBar.date.getDate(),
      };

      let old_price = nextBar.bar.close;
      let volatility = 0.1;
      let rnd = Math.random();
      let change_percent = 2 * volatility * rnd;

      if (change_percent > volatility) change_percent -= 2 * volatility;

      let change_amount = old_price * change_percent;
      nextBar.bar.open = nextBar.bar.close;
      nextBar.bar.close = old_price + change_amount;
      nextBar.bar.high =
        Math.max(nextBar.bar.open, nextBar.bar.close) +
        Math.abs(change_amount) * Math.random();
      nextBar.bar.low =
        Math.min(nextBar.bar.open, nextBar.bar.close) -
        Math.abs(change_amount) * Math.random();
      nextBar.bar.value = Math.random() * 100;
      nextBar.bar.color =
        nextBar.bar.close < nextBar.bar.open
          ? "rgba(255, 128, 159, 0.25)"
          : "rgba(107, 255, 193, 0.25)";

      return nextBar.bar;
    },
  },
  mounted() {
    this.chart = createChart("lightweight-chart", this.chartConfig);

    this.candleSeries = this.chart.addCandlestickSeries(this.candlesConfig);

    this.volumeSeries = this.chart.addHistogramSeries();

    for (let i = 0; i < 150; i++) {
      const bar = this.setNextBar();
      this.candleSeries.update(bar);
      this.volumeSeries.update(bar);
    }

    this.controlResponsive();

    setInterval(() => {
      const bar = this.setNextBar();
      this.candleSeries.update(bar);
      this.volumeSeries.update(bar);
    }, 3000);

    window.addEventListener("resize", this.controlResponsive, false);
    this.setNextBar();
  },
};
</script>
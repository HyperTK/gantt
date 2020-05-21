<template>
  <div id="app">
    <GSTC :config="config" @state="onState" />
  </div>
</template>

<script>
import GSTC from "vue-gantt-schedule-timeline-calendar";
let subs = [];

export default {
  name: "app",
  components: {
    GSTC,
  },
  data() {
    return {
      config: {
        height: 500,
        list: {
          rows: {
            "NMV3000DCG-1": {
              id: "NMV3000DCG-1",
            },
            "NMV3000DCG-2": {
              id: "NMV3000DCG-2",
            },
            "NMV3000DCG-3": {
              id: "NMV3000DCG-3",
            },
            "NMV5000DCG-1": {
              id: "NMV5000DCG-1",
            },
            A00000F: {
              id: "A00000F",
              sid: "1",
              label: "S0123-44-569",
              parentId: "NMV3000DCG-1",
            },
            A00001F: {
              id: "A00001F",
              sid: "2",
              label: "S0123-44-569",
              parentId: "NMV3000DCG-2",
            },
            A00002F: {
              id: "A00002F",
              sid: "3",
              label: "S0123-44-569",
              parentId: "NMV3000DCG-3",
            },
            A00003F: {
              id: "A00003F",
              sid: "4",
              label: "S0123-44-569",
              parentId: "NMV3000DCG-3",
            },
          },
          columns: {
            data: {
              id: {
                id: "id",
                data: "id",
                width: 200,
                header: {
                  content: "工程",
                },
                expander: true,
              },
              label: {
                id: "label",
                data: "label",
                width: 150,
                header: {
                  content: "図番",
                },
              },
              // スプレッドシートに紐付けるID
              sid: {
                id: "sid",
                data: "sid",
                width: 0,
                header: {
                  content: "シートID",
                },
              },
            },
          },
        },
        chart: {
          items: {
            "1": {
              id: "1",
              rowId: "A00000F",
              label: "Item 1",
              time: {
                start: new Date().getTime(),
                end: new Date().getTime() + 24 * 60 * 60 * 1000,
              },
            },
            "2": {
              id: "2",
              rowId: "2",
              label: "Item 2",
              time: {
                start: new Date().getTime() + 4 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 5 * 24 * 60 * 60 * 1000,
              },
            },
            "3": {
              id: "3",
              rowId: "2",
              label: "Item 3",
              time: {
                start: new Date().getTime() + 6 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 7 * 24 * 60 * 60 * 1000,
              },
            },
            "4": {
              id: "4",
              rowId: "3",
              label: "Item 4",
              time: {
                start: new Date().getTime() + 10 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 12 * 24 * 60 * 60 * 1000,
              },
            },
            "5": {
              id: "5",
              rowId: "4",
              label: "Item 5",
              time: {
                start: new Date().getTime() + 12 * 24 * 60 * 60 * 1000,
                end: new Date().getTime() + 14 * 24 * 60 * 60 * 1000,
              },
            },
          },
        },
      },
    };
  },
  methods: {
    onState(state) {
      this.state = state;
      subs.push(
        state.subscribe("config.chart.items.1", (item) => {
          console.log("item 1 changed", item);
        })
      );
      subs.push(
        state.subscribe("config.list.rows.1", (row) => {
          console.log("row 1 changed", row);
        })
      );
    },
  },
  mounted() {
    setTimeout(() => {
      const item1 = this.config.chart.items["1"];
      item1.label = "label changed dynamically";
      item1.time.end += 2 * 24 * 60 * 60 * 1000;
    }, 2000);
  },
  beforeDestroy() {
    subs.forEach((unsub) => unsub());
  },
};
</script>

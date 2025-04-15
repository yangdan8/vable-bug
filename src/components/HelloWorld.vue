<template>
  <div class="pivot-table" />
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import {
  PivotTable,
} from "@visactor/vue-vtable";
import {
  type PivotTableConstructorOptions,
} from "@visactor/vtable";
import type { IDimensionInfo } from "@visactor/vtable/es/ts-types";

onMounted(() => {
  console.log("Hello World!", PivotTable);
  fetch(
    "https://lf9-dp-fe-cms-tos.byteorg.com/obj/bit-cloud/VTable/North_American_Superstore_Pivot_Chart_data.json"
  )
    .then((res) => res.json())
    .then((data) => {
      console.log("fetch-data", data);
      const tableOptions: PivotTableConstructorOptions = {
        records: data,
        rows: [
          {
            dimensionKey: "Category",
            title: "Category",
            headerStyle: {
              textStick: true,
              bgColor(arg) {
                if (arg.dataValue === "Row Totals") {
                  return "#ff9900";
                }
                return "#ECF1F5";
              },
            },
            width: "auto",
          },
          {
            dimensionKey: "Sub-Category",
            title: "Sub-Catogery",
            headerStyle: {
              textStick: true,
            },
            width: "auto",
          },
        ],
        columns: [
          {
            dimensionKey: "Region",
            title: "Region",
            headerStyle: {
              textStick: true,
            },
            // width: "auto",
          },
          {
            dimensionKey: "Segment",
            title: "Segment",
            headerStyle: {
              textStick: true,
            },
            // width: "auto",
          },
        ],
        indicators: [
          {
            indicatorKey: "Quantity",
            title: "Quantity",
            width: "auto",
            sort: true,
            headerStyle: {
              fontWeight: "normal",
            },
            style: {
              padding: [16, 28, 16, 28],
              color(args) {
                if (args.dataValue >= 0) return "black";
                return "red";
              },
              bgColor(arg) {
                const rowHeaderPaths = arg.cellHeaderPaths
                  ?.rowHeaderPaths as IDimensionInfo[];
                if (rowHeaderPaths?.[1]?.value === "Sub Totals") {
                  return "#ba54ba";
                } else if (rowHeaderPaths?.[0]?.value === "Row Totals") {
                  return "#ff9900";
                }
                return "";
              },
            },
          },
          {
            indicatorKey: "Sales",
            title: "Sales",
            width: "auto",
            sort: true,
            headerStyle: {
              fontWeight: "normal",
            },
            format: (rec) => {
              return "$" + Number(rec).toFixed(2);
            },
            style: {
              padding: [16, 28, 16, 28],
              color(args) {
                if (args.dataValue >= 0) return "black";
                return "red";
              },
              bgColor(arg) {
                const rowHeaderPaths = arg.cellHeaderPaths
                  ?.rowHeaderPaths as IDimensionInfo[];
                if (rowHeaderPaths?.[1]?.value === "Sub Totals") {
                  return "#ba54ba";
                } else if (rowHeaderPaths?.[0]?.value === "Row Totals") {
                  return "#ff9900";
                }
                return "";
              },
            },
          },
          {
            indicatorKey: "Profit",
            title: "Profit",
            width: "auto",
            showSort: false,
            headerStyle: {
              fontWeight: "normal",
            },
            format: (rec) => {
              return "$" + Number(rec).toFixed(2);
            },
            style: {
              padding: [16, 28, 16, 28],
              color(args) {
                if (args.dataValue >= 0) return "black";
                return "red";
              },
              bgColor(arg) {
                const rowHeaderPaths = arg.cellHeaderPaths
                  ?.rowHeaderPaths as IDimensionInfo[];
                if (rowHeaderPaths?.[1]?.value === "Sub Totals") {
                  return "#ba54ba";
                } else if (rowHeaderPaths?.[0]?.value === "Row Totals") {
                  return "#ff9900";
                }
                return "";
              },
            },
          },
        ],
        corner: {
          titleOnDimension: "all",
        },
        widthMode: "standard",
      };
      const tableInstance = new PivotTable(
        document.querySelector(".pivot-table") as HTMLElement,
        tableOptions
      );
    });
});
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

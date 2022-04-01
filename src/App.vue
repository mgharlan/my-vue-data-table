<template>
  <h2>Testing Empty Rows & Columns</h2>
  <v-data-table :columns="[]" :rows="[]" :per-page="10" />
  <br>
  <v-data-table :columns="[]" :rows="[]" :per-page="1" />
  <br>
  <v-data-table :columns="[]" :rows="[]" :per-page="0" />
  <br>
  <h2>Testing Per Page Sizes</h2>
  <v-data-table :columns="columns" :rows="[]" :per-page="4" />
  <br>
  <v-data-table :columns="columns" :rows="campaigns" :per-page="1" />
  <br>
  <v-data-table :columns="columns" :rows="campaigns" :per-page="3" />
  <br>
  <v-data-table :columns="columns" :rows="campaigns" :per-page="10" />
  <br>
  <h2>Testing Larger Result Set</h2>
  <v-data-table :columns="biggerColumns" :rows="biggerData" :per-page="10" />
</template>

<script>
  import VDataTable from "@/components/VDataTable.vue";
  import { format } from "date-fns";

  export default{
    components: {
      VDataTable
    },
    data() {
      return {
        columns: [
          {
            dataKey: "code",
            name: "Code",
            align: "left",
          },
          {
            dataKey: "startDate",
            name: "Start Date",
            align: "left",
            formatValue(value) {
              return format(value, "dd MMMM yyyy")
            }
          },
        ],
        campaigns: [
          {
            code: "CAM1",
            startDate: new Date(2019, 2, 15),
          },    
          {
            code: "CAM2",
            startDate: new Date(2019, 2, 19),
          },
          {
            code: "CAM3",
            startDate: new Date(2019, 2, 18),
          },
          {
            code: "CAM4",
            startDate: new Date(2019, 2, 17),
          },
          {
            code: "CAM5",
            startDate: new Date(2019, 2, 17),
          },
        ],
        biggerColumns: [
         {
            dataKey: "code",
            name: "Code",
            align: "left",
          },
          {
            dataKey: "media",
            name: "Media",
            align: "center",
          },
          {
            dataKey: "startDate",
            name: "Start Date",
            align: "left",
            formatValue(value) {
              return format(value, "dd MMMM yyyy")
            }
          }, 
          {
            dataKey: "lastOrder",
            name: "Last Order",
            align: "left",
            formatValue(value) {
              return format(value, "dd MMMM yyyy")
            }
          },
          {
            dataKey: "sale",
            name: "Sale",
            align: "right",
            formatValue(value) {
              return "£" + value;
            }
          },
        ],
        biggerData: this.getBiggerData(),
      }
    },
    methods: {
      getBiggerData(){
        let biggerData = [];
        for(let i = 0; i < 100; i++){
          biggerData.push(
            {
              code: "TEST" + i,
              media: "Media Info",
              startDate: new Date(2019 - i, 3, i%31 + 1),
              lastOrder: new Date(2022, i%12 + 1, i%31 + 1),
              sale: 100 * i,
            }
          );
        }
        return biggerData;
      }
    }
  }
</script>

<style scoped>
  h2{
    font-family:Arial, Helvetica, sans-serif;
  }
</style>

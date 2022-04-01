<template>
  <v-data-table :columns="columns" :rows="campaigns" :per-page="7" />
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
        ]
      }
    }
  }
</script>

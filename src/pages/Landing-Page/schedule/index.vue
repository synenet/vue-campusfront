<template>
  <Table :data="table" />
</template>

<script>
import { reactive } from "vue";
import { onBeforeMount } from "vue";
import { UserService } from "@/services";
import Table from "@/components/table/index.vue";
export default {
  name: "TakenCoursesTableView",
  components: {
    Table,
  },
  setup() {
    const table = reactive({
      rows: [
        {
          days: "M",
        },
        {
          days: "T",
        },
        {
          days: "W",
        },
        {
          days: "Th",
        },
        {
          days: "F",
        },
        {
          days: "Sa",
        },
        {
          days: "Su",
        },
      ],
    });

    onBeforeMount(async () => {
      const res = await UserService.course.get();
      for (let i = 0; i < res.data.length; i++) {
        const days = res.data[i].courseDays;
        const hours = res.data[i].courseHours;
        if (table.rows[days][hours]) {
          table.rows[days][hours] += " " + res.data[i].courseCode;
        } else {
          table.rows[days][hours] = res.data[i].courseCode;
        }
      }
    });
    return {
      table,
    };
  },
};
</script>

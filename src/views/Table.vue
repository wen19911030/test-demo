<template>
  <div class="panel">
    <basic-table :table-data="tableData" :columns="columns" :width="width"></basic-table>
    <el-table :data="tableData" stripe>
      <el-table-column prop="date" label="日期" width="180" fixed></el-table-column>
      <el-table-column prop="name" label="姓名" width="180"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
    </el-table>
  </div>
</template>

<script>
import BasicTable from "@/components/BasicTable.vue";

export default {
  name: "v-table",
  components: {
    BasicTable
  },
  data() {
    return {
      width: "500px",
      columns: [
        {
          label: "姓名",
          field: "name",
          isFixed: true
        },
        {
          label: "年龄",
          field: "age",
          width: "60px"
        },
        {
          label: "地址",
          field: "address"
        },
        {
          label: "日期",
          field: "date"
        }
      ],
      tableData: [
        {
          name: "John Brown",
          age: 14,
          address: "New York No. 1 Lake Park",
          date: "2016-10-03"
        },
        {
          name: "Jim Green",
          age: 24,
          address: "London No. 1 Lake Park",
          date: "2016-10-01"
        },
        {
          name: "Joe Black",
          age: 30,
          address: "Sydney No. 1 Lake Park",
          date: "2016-10-02"
        },
        {
          name: "Jon Snow",
          age: 17,
          address: "Ottawa No. 2 Lake Park",
          date: "2016-10-04"
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>
</style>

<template>
  <div>
    <div class="table-container flex">
      <div class="table-header-wrapper" ref="headerWrapper">
        <table>
          <colgroup>
            <col v-for="(item, index) in tableHeaderData" :key="index" :align="item.headerAlign">
          </colgroup>
          <thead>
            <tr>
              <th
                v-for="(item, index) in tableHeaderData"
                :key="index"
                :class="{'is-hidden': item.isFixed}"
              >
                <div class="cell">{{item.label}}</div>
              </th>
            </tr>
          </thead>
        </table>
      </div>

      <div class="table-body-wrapper" ref="bodyWrapper">
        <table>
          <colgroup>
            <col v-for="(item, index) in tableHeaderData" :key="index" :align="item.align">
          </colgroup>
          <tbody>
            <tr v-for="(item, index) in tableBodyData" :key="index">
              <td
                v-for="(one, i) in item"
                :key="i"
                :class="{'is-hidden': one.isFixed}"
              >{{one.value}}</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="table-left-wrapper" v-if="isLeftFixed" ref="leftFixedWrapper">
        <div class="table-left-fixed-header-wrapper" ref="fixedHeaderWrapper">
          <table>
            <colgroup>
              <col v-for="(item, index) in tableHeaderData" :key="index" :align="item.headerAlign">
            </colgroup>
            <thead>
              <tr>
                <th
                  v-for="(item, index) in tableHeaderData"
                  :key="index"
                  :class="{'is-hidden': !item.isFixed}"
                >
                  <div class="cell">{{item.label}}</div>
                </th>
              </tr>
            </thead>
          </table>
        </div>
        <div class="table-left-fixed-body-wrapper" ref="fixedBodyWrapper">
          <table>
            <colgroup>
              <col v-for="(item, index) in tableHeaderData" :key="index" :align="item.align">
            </colgroup>
            <tbody>
              <tr v-for="(item, index) in tableBodyData" :key="index">
                <td
                  v-for="(one, i) in item"
                  :key="i"
                  :class="{'is-hidden': !one.isFixed}"
                >{{one.value}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <!-- <div class="table-left-wrapper" v-if="isLeftFixed" ref="rightFixedWrapper"></div> -->
  </div>
</template>

<script>
export default {
  name: "basic-table",
  props: {
    tableData: {
      type: Array
    },
    columns: {
      type: Array,
      default: () => {
        return [
          {
            label: "",
            field: "",
            isFixed: false,
            width: "",
            textAlign: "",
            formatter: () => {}
          }
        ];
      }
    },
    width: {
      type: String
    }
  },
  data() {
    return {
      isLeftFixed: false,
      isRightFixed: false,
      tableHeaderData: [],
      tableBodyData: [],
      leftTableObj: {
        thead: [],
        tbody: []
      },
      centerTableObj: {
        thead: [],
        tbody: []
      },
      rightTableObj: {
        thead: [],
        tbody: []
      }
    };
  },
  watch: {
    columns: {
      handler: function(newV) {
        this.createTable();
      },
      immediate: true,
      deep: true
    },
    tableData: {
      handler: function(newV) {
        this.createTable();
      },
      deep: true
    }
  },
  methods: {
    createTable() {
      this.tableHeaderData = this.columns.map(item => {
        let obj = Object.assign(
          {
            headerAlign: "left",
            align: "left"
          },
          item
        );
        return obj;
      });

      this.tableBodyData = this.tableData.map(item => {
        let arr = this.tableHeaderData.map(one => {
          one.value = item[one.field];
          return one;
        });
        return arr;
      });

      let first = this.columns[0],
        last = this.columns[this.columns.length - 1],
        start = 0,
        end = Number.MAX_SAFE_INTEGER;
      this.isLeftFixed = !!first.isFixed;
      if (this.columns.length > 1) {
        this.isRightFixed = !!last.isFixed;
      } else {
        this.isRightFixed = false;
      }
      if (this.isLeftFixed) {
        start = 1;
        this.leftTableObj.thead = [first];
        this.leftTableObj.tbody = this.tableData.map(item => {
          let obj = Object.assign({}, first);
          obj.value = item[obj.field];
          return obj;
        });
      }
      if (this.isRightFixed) {
        end = -1;
        this.rightTableObj.thead = [last];
        this.rightTableObj.tbody = this.tableData.map(item => {
          let obj = Object.assign({}, last);
          obj.value = item[obj.field];
          return obj;
        });
      }
      let centerArr = this.columns.slice(start, end);
      this.centerTableObj.thead = centerArr.slice(0);
      this.centerTableObj.tbody = this.tableData.map(item => {
        return this.centerTableObj.thead.map(one => {
          let obj = Object.assign({}, one);
          obj.value = item[obj.field];
          return obj;
        });
      });
    }
  }
};
</script>

<style lang="scss" scoped>
</style>

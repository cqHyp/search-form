<template>
  <div class="home">
    <search-form :query-list="queryList" @handleQuery="handleQuery" @resetQuery="resetQuery"></search-form>
  </div>
</template>

<script>
// @ is an alias to /src
import SearchForm from "../components/search-form";

export default {
  name: 'Home',
  components: {
    SearchForm
  },
  data() {
    return {
      queryList: [
        {
          id: "machineId",
          name: "machineId",
          label: "设备ID",
          tips: "请输入设备ID",
          type: "input",
          value: "",
        },
        {
          id: "category",
          name: "category",
          label: "设备类型",
          tips: "请选择设备类型",
          type: "select",
          value: "",
          options: [
            {label: "CNC", value: "CNC"},
            {label: "IMM", value: "IMM"}
          ],
        },
        {
          id: "createDateRange",
          name: "createDateRange",
          label: "创建时间",
          type: "datePicker",
          format: "yyyy-MM-dd HH:mm:ss",
          separator: "-",
          dateType: "datetimerange",
          style: "width: 340px",
          startPlaceholder: "开始",
          endPlaceholder: "结束",
          value: "",
          propName: ["createDateStart", "createDateEnd"],
          defaultTime: ["00:00:00", "23:59:59"]
        },
      ],
    }
  },
  methods: {
    handleQuery: function () {
      let params = this.buildSearchQuery(this.queryList);
      console.log(params);
    },
    resetQuery: function () {
      this.queryList.forEach((item) => {
        item.value = "";
      });
    },
    buildSearchQuery(array) {
      let obj = {}
      array.forEach(item => {
        if (null !== item.value && '' !== item.value) {
          if (item.type === 'datePicker') {
            if (item.dateType === "date") {
              obj[item.id] = item.value;
            } else {
              if (null !== item.value && '' !== item.value) {
                if (item.propName) {
                  obj[item.propName[0]] = item.value[0];
                  obj[item.propName[1]] = item.value[1];
                } else {
                  obj['beginTime'] = item.value[0];
                  obj['endTime'] = item.value[1];
                }
              }
            }
          } else {
            obj[item.id] = item.value
          }
        }
      })
      return obj
    }
  }
}
</script>

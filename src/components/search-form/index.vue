<template>
  <div class="search-form-wrapper">
    <div class="search-item" v-for="(item, index) in queryList" :key="item.id + '_query_' + index">
      <div class="search-label">{{item.label}}</div>
      <div class="search-inner">
        <component
            clearable
            size="small"
            :style="item.style ? item.style : 'width: 240px;'"
            :id="item.id"
            :name="item.name"
            v-bind:is="componentList.filter(it => {return it.type === item.type})[0].component"
            :placeholder="item.tips"
            v-model="item.value"
            :value-format="item.format"
            :type="item.dateType"
            :range-separator="item.separator"
            :start-placeholder="item.startPlaceholder"
            :end-placeholder="item.endPlaceholder"
            :default-time="item.defaultTime"
        >
          <el-option v-if="item.type === 'select'" v-for="(it, ind) in item.options"
                     :key="item.id + '_option_' + it.value" :label="it.label" :value="it.value"></el-option>
        </component>
      </div>
    </div>
    <el-button style="margin-bottom: 12px;" type="primary" icon="el-icon-search" size="mini" @click="handleQuery">搜索</el-button>
    <el-button style="margin-bottom: 12px;" icon="el-icon-refresh" size="mini" @click="resetQuery">重置</el-button>
  </div>
</template>

<script>
import { Input, Select, DatePicker } from 'element-ui'

export default {
  name: 'SearchForm',
  props: {
    queryList: {
      type: Array,
      require: true,
      default: []
    }
  },
  data() {
    return {
      componentList: [
        { type: 'input', component: Input },
        { type: 'select', component: Select },
        { type: 'datePicker', component: DatePicker }
      ]
    }
  },
  mounted: function() {

  },
  methods: {
    handleQuery: function() {
      this.$emit("handleQuery");
    },
    resetQuery: function() {
      this.$emit("resetQuery");
    }
  }
}
</script>

<style lang="scss" scoped>
.search-form-wrapper {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;

  .search-item {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 12px;
    margin-right: 8px;

    .search-label {
      font-size: 14px;
      color: #606266;
      min-width: 5em;
      margin-right: 16px;
      text-align: right;
      font-weight: bold;
    }

    .search-inner {
      /*min-width: 240px;*/
    }
  }
}
</style>

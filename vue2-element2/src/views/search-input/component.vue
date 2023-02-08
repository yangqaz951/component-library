<template>
  <div class="main">
    <el-form
      :model="queryParams"
      :label-position="labelPosition"
      ref="queryForm"
      :inline="true"
      class="query-form"
      :rules="rules"
    >
      <el-form-item
        v-for="(item, index) in formData"
        :key="index"
        :prop="item.prop"
        :label="item.name"
        v-show="!(item.hide && !nQueryExpand)"
      >
        <el-input
          v-model="item.value"
          :placeholder="item.placeholder"
          clearable
          :suffix-icon="item.icon"
          maxlength="32"
          :size="size"
          v-if="item.type == 'text'"
        ></el-input>
        <el-select
          v-model="item.value"
          :size="size"
          :placeholder="item.placeholder"
          :multiple="item.multiple"
          v-else-if="item.type == 'select'"
        >
          <el-option
            v-for="item in item.data"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
        <el-row v-else-if="item.type == 'section'">
          <el-col :span="10">
            <el-input
              :size="size"
              @input="balanceBlur('value1', index)"
              :placeholder="item.placeholder1"
              v-model.number="item.value1"
              type="text"
            />
          </el-col>
          <el-col :span="4" style="text-align: center"> - </el-col>
          <el-col :span="10">
            <el-input
              :size="size"
              @input="balanceBlur('value2', index)"
              :placeholder="item.placeholder2"
              v-model.number="item.value2"
              type="text"
            />
          </el-col>
        </el-row>
        <el-date-picker
          v-else-if="item.type == 'daterange'"
          v-model="item.value"
          type="daterange"
          range-separator="-"
          size="small"
          style="width: 100%"
          :start-placeholder="item.placeholder1"
          :end-placeholder="item.placeholder2"
        >
        </el-date-picker>
      </el-form-item>
    </el-form>
    <div class="scrm-search">
      <div>
        <el-button
          class="scrm-buttom"
          icon="el-icon-search"
          type="primary"
          @click="getFormData"
          >查询</el-button
        >
        <el-button
          class="scrm-buttom"
          icon="el-icon-refresh"
          @click="clickReset"
          >重置</el-button
        >
      </div>
      <el-button
        class="customer-expand-button"
        type="text"
        :size="size"
        :icon="nQueryExpand ? 'el-icon-arrow-up' : 'el-icon-arrow-down'"
        @click="() => (nQueryExpand = !nQueryExpand)"
      >
        {{ nQueryExpand ? "收起" : "展开" }}
      </el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: "search-input",
  props: {
    // 表单大小
    size: {
      type: String,
      default: "small",
    },
    // 对齐方式
    labelPosition: {
      type: String,
      default: "top",
    },
    // 表单数据
    formData: {
      type: Array,
    },
    // 数据校验
    rules: {
      type: Object,
    },
  },
  data() {
    return {
      // 传参数
      queryParams: {},
      // 展开/收起搜索
      nQueryExpand: false,
    };
  },
  create() {},
  methods: {
    balanceBlur(val, index) {
      this.formData[index][val] = Number(this.formData[index][val])
        ? Number(this.formData[index][val])
        : "";
    },
    getFormData() {
      // for(let key in this.formData) {
      //   if(this.formData[key].type=='section'&&(this.formData[key].value1||this.formData[key].value2)){
      //     this.queryParams[this.formData[key].prop] = this.formData[key].value1 + '_' + this.formData[key].value2
      //   }else{
      //     this.queryParams[this.formData[key].prop] = this.formData[key].value
      //   }
      // }
      this.formData.length > 0 &&
        this.formData.forEach((item) => {
          if (item.type == "section" && (item.value1 || item.value2)) {
            this.queryParams[item.prop] = item.value1 + "_" + item.value2;
          } else {
            this.queryParams[item.prop] = item.value;
          }
        });
      console.log(this.queryParams);
      this.$emit("getFormData", this.queryParams);
    },
    clickReset() {
      console.log(this.formData);
    },
  },
};
</script>

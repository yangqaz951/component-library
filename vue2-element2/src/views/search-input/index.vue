<template>
  <div class="main">
    <SearchInput
      :rules="rules"
      :formData="formData"
      @getFormData="getFormData"
    />
  </div>
</template>

<script>
import SearchInput from "./component";
export default {
  name: "search-input",
  components: {
    SearchInput,
  },
  data() {
    return {
      rules: {
        customerName: [
          { required: true, message: "请输入活动名称", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" },
        ],
      },
      // 表单数据
      formData: [
        {
          // 标题
          name: "文本",
          // 输入框类型
          type: "text",
          // 输入框绑定的值
          value: "",
          // icon
          icon: "",
          // 是否隐藏
          hide: "false",
          // 字段名称
          prop: "customerName",
          placeholder: "请输入",
        },
        {
          name: "日期范围",
          type: "daterange",
          value: "",
          icon: "el-icon-date",
          prop: "addWxTime",
          placeholder1: "请输入1",
          placeholder2: "请输入2",
        },
        {
          name: "数字范围",
          type: "section",
          value: "",
          icon: "search",
          hide: "true",
          prop: "pointsSumDealer",
          value1: "",
          value2: "",
          placeholder1: "请输入1",
          placeholder2: "请输入2",
        },
        {
          name: "下拉单选",
          type: "select",
          multiple: true,
          value: "",
          icon: "search",
          hide: "true",
          prop: "pointsSum",
          placeholder: "请选择",
          data: [
            {
              label: "牛",
              value: "1",
            },
            {
              label: "羊",
              value: "2",
            },
            {
              label: "马",
              value: "3",
            },
          ],
        },
      ],
      // 传参数
      queryParams: {},
      // 展开/收起搜索
      nQueryExpand: false,
    };
  },
  methods: {
    getFormData(data) {
      // for(let key in this.formData) {
      //   if(this.formData[key].type=='section'&&(this.formData[key].value1||this.formData[key].value2)){
      //     this.queryParams[this.formData[key].prop] = this.formData[key].value1 + '_' + this.formData[key].value2
      //   }else{
      //     this.queryParams[this.formData[key].prop] = this.formData[key].value
      //   }
      // }
      console.log(data);
    },
    clickReset() {
      console.log(this.formData);
    },
  },
};
</script>

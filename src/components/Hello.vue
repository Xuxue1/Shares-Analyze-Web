<template>
  <div>
    <div class="searchWrapper marginBtm20">
      <Date-picker v-on:on-change="dateChange" type="date" placeholder="选择日期" style="width: 220px"></Date-picker>
      <Button @click="search">查询</Button>
      <div class="btnWrapper">
        <Button type="primary" size="large" @click="exportData(1)"><Icon type="ios-download-outline"></Icon> 导出原始数据</Button>
        <Button type="primary" size="large" @click="exportData(2)"><Icon type="ios-download-outline"></Icon> 导出排序和过滤后的数据</Button>
        <Button type="primary" size="large" @click="exportData(3)"><Icon type="ios-download-outline"></Icon> 导出自定义数据</Button></div>
    </div>
    <div class="tableWrapper marginBtm20">
      <Table :columns="columns" :data="data" size="small" ref="table"></Table>
    </div>
  </div>
</template>

<script>
  import VueResource from 'vue-resource'
  import Vue from 'vue'
  Vue.use(VueResource);
  export default {
    data () {
      return {
        columns: [
          {
            "title": "公司名称",
            "key": "company",
            "fixed": "left",
            "width": 200
          },
          {
            "title": "微信公众号名称",
            "key": "nickName",
            "width": 150,
            "sortable": true,
          },
          {
            "title": "时间",
            "key": "date",
            "width": 150,
            "sortable": true
          }
        ],
        data: [],
        time:'2017-8-24'
      }
    },
    created(){
      var url = "/api/result?date="+this.time
      this.$http.get(url).then(response=>{
        this.data = response.data;
      },response=>{
        console.log("error request!")
      });
    },
    methods: {
      dateChange(date){
        this.time = date
        console.log("date change "+this.time)
      },
      search(){
        var url = "/api/result?date="+this.time;
        this.$http.get(url).then(response=>{
          this.data = response.data;
        },response=>{
          console.log("error request!")
        });
      },
      exportData (type) {
        if (type === 1) {
          this.$refs.table.exportCsv({
            filename: '原始数据'
          });
        } else if (type === 2) {
          this.$refs.table.exportCsv({
            filename: '排序和过滤后的数据',
            original: false
          });
        } else if (type === 3) {
          this.$refs.table.exportCsv({
            filename: '自定义数据',
            columns: this.columns8.filter((col, index) => index < 4),
            data: this.data7.filter((data, index) => index < 4)
          });
        }
      }
    }
  }
</script>

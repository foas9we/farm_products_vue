<template>
    <div class = "product_list">
        <!-- 按钮 -->
       <div class = "btns">
           <el-button type="primary" size="small" @click="toPulishProduct">发布农产品</el-button>
       </div>
       <!-- 表格 -->
     <el-table :data="product"  style="width: 100%">
      <el-table-column  prop="id" label="编号" width="180"></el-table-column>
      <el-table-column prop="title" label="标题" width="180"></el-table-column>
      <el-table-column prop="price" label="价格"> </el-table-column>
      <el-table-column prop="media" label="url地址"> </el-table-column>
      <el-table-column prop="state" label="农产品属性"> </el-table-column>
      <el-table-column prop="categoryId" label="父栏目编号"> </el-table-column>
      <el-table-column prop="userId" label="所有者编号"> </el-table-column>
      <el-table-column prop="description" label="描述信息"> </el-table-column>
    </el-table>
       <!-- 分页 -->
    </div>
</template>
<script>
import request from '@/utils/request'
export default {
    data(){
        return{
            product:[]
        }
   },

   created(){
       request.get('http://localhost:8848/product/findAllProduct')
       .then(result=>{
          this.product = result.data;
       })
   },
   methods:{
       toPulishProduct(){
           //跳转到编辑农产品界面
           this.$router.push({path:'/Product/Editor'})
       }
   }
}
</script>
<style scoped>

</style>
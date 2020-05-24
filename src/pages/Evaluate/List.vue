<template>
    <div class = "evaluate_list">
      <div class="search" >
                  <el-select v-model="value" multiple  filterable="true" remote="true" reserve-keyword
                      placeholder="请输入关键词"
                      :remote-method="searchFor"
                      :loading="loading"
                  >
                      <el-option  v-for="item in options" :key="item.id" :label="item.title" :value="item">
                      </el-option>
                  </el-select>
              </div>
       <!-- 表格 -->
     <el-table :data="evaluate"  style="width: 100%">
      <el-table-column  prop="id" label="编号" width="180"></el-table-column>
      <el-table-column prop="content" label="评论" width="180"></el-table-column>
      <el-table-column prop="date" label="评论时间"> </el-table-column>
      <el-table-column  label="图片">
          <template scope="scope" >
            <img :src="scope.row.picture" width="40"  v-if="scope.row.picture!=null" height="40"/>
          </template>
      </el-table-column>
      <el-table-column prop="user.name" label="评论人"> </el-table-column>
      <el-table-column prop="productId" label="产品编号"> </el-table-column>
       <el-table-column
      fixed="right"
      label="操作"
      align="center"
      width="150">
      <template slot-scope="scope">
        <el-button @click="toDelete(scope.row.id)" type="text" size="small">撤回</el-button>
      </template>
    </el-table-column>
    </el-table>
       <!-- 分页 -->
    </div>
</template>
<script>
import request from '@/utils/request'
export default {
    data(){
        return{
            evaluate:[],
            options:[],
            props:{multiple: true ,label:'name',value:'id',emitPath:false},
        }
   },

   created(){
      this.reloadData();
   },
   methods:{
     searchFor(value){
        request.get("/evaluate/cascadeFindByUserName?name="+value)
                  .then(response=>{
                    this.evaluate = response.data;

                    })
     },
       reloadData(){
                request.get('/evaluate/cascadeFindAll')
        .then(result=>{
            this.evaluate = result.data;
        })
       },
       toDelete(id){
          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          //交互
              let url = "/evaluate/deleteById"
              request.get(url,{params:{id:id}})
              .then(response=>{
                  //通知
                  this.$message({
                      message:response.message,
                      type:"success"
                  })
                  //重载数据
                  this.reloadData();
              })

        })
       }

   }
}
</script>
<style scoped>

</style>

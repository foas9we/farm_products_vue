<template>
    <div class="product_editor">
        <el-button type="text" @click="back">返回</el-button>
        {{form}}
        <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="发布类型">
                <el-select v-model="form.state" placeholder="请选择发布类型">
                    <el-option label="发布采购信息" value="2"></el-option>
                    <el-option label="发布供货信息" value="1"></el-option>
                </el-select>
        </el-form-item>
        <el-form-item label="标题">
            <el-input v-model="form.title"></el-input>
        </el-form-item>
        <el-form-item label="价格">
            <el-input v-model="form.price"></el-input>
        </el-form-item>
        <el-form-item label="描述信息">
            <el-input type="textarea" v-model="form.description" rows="5"></el-input>
        </el-form-item>
        <el-form-item label="类别">
        <el-select v-model="form.category_id" placeholder="请选择所属栏目">
            <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
            </el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="图片详情">
            <el-upload
                class="upload-demo"
                action="https://jsonplaceholder.typicode.com/posts/"
                :on-preview="handlePreview"
                :on-remove="handleRemove"
                :before-remove="beforeRemove"
                multiple
                :limit="3"
                :on-exceed="handleExceed"
                :file-list="fileList">
                <el-button size="small" type="primary">点击上传</el-button>
                <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </el-upload>
        </el-form-item>
  
  <el-form-item>
    <el-button type="primary" @click="onSubmit">立即发布</el-button>
    <el-button>取消</el-button>
  </el-form-item>
</el-form>
    </div>
</template>

<script>
import request from '@/utils/request'
import qs from 'qs'
export default {
    data(){
        return{
            form:{}
        }
    },
    methods:{
        back(){
            this.$router.go(-1);
        },
        onSubmit(){
            request(
                {
                    method:"post",
                    url:'http://localhost:8848/product/saveOrUpdateProduct',
                    data:qs.stringify(this.form),
                    headers:{
                        'Content-Type':'application/x-www-form-urlencoded'
                    }
                }
            ).then(result=>{
        this.$message({
            message:result.message,
            type:"success"
        });
        this.back();
      })
        }
    }
}
</script>
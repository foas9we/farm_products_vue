<template>
    <div class="privilege_list">
        <div class="btns">
            <el-button type="primary" size="small">新增权限</el-button>
        </div>
        
        <el-table :data="privileges" size = "small" row-key="id">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="权限名"></el-table-column>
            <el-table-column prop="type" label="类型"></el-table-column>
            <el-table-column prop="route" label="路径"></el-table-column>
            
            <el-table-column label="操作" aling="center" width="100">
                <template v-slot="slot">
                    <el-button size="mini" type = "text" @click="toBindPrivilege(slot.row)">修改</el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>
<script>
import request from '@/utils/request'
export default {
    data(){
        return{
            privileges:[]
        }
    },
    created(){
        this.loadPrivileges();
    },
    methods:{
        //加载权限信息
        loadPrivileges(){
            request.get("/privilege/findAllWithChildren")
            .then(response=>{
                this.privileges = response.data;
            })
        },
        //绑定权限
        toBindPrivilege(privilege){
            alert(JSON.stringify(privilege));    
        }
    }
}
</script>
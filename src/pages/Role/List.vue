<template>
    <div class="role_list">
        <div class="btns">
            <el-button type="primary" size="small">新增角色</el-button>
        </div>
        <el-table :data="roles" size = "small">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="角色名"></el-table-column>
            
            <el-table-column label="操作" aling="center" width="100">
                <template v-slot="slot">
                    <el-button size="mini" type = "text" @click="toBindPrivilege(slot.row)">授权</el-button>
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
            roles:[]
        }
    },
    created(){
        this.loadRoles();
    },
    methods:{
        //加载角色信息
        loadRoles(){
            request.get("/role/findAll")
            .then(response=>{
                this.roles = response.data;
            })
        },
        //绑定角色
        toBindPrivilege(role){
            alert(JSON.stringify(role));    
        }
    }
}
</script>
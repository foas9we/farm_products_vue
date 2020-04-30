<template>
    <div class="user_list">
        <!-- 按钮 -->
        <div class="btns">
            <el-button type="primary" size="small">新增用户</el-button>
        </div>
        <!-- 表格 -->
        <el-table :data="users" size = "small">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="用户名"></el-table-column>
            <el-table-column prop="phone" label="手机号"></el-table-column>
            <el-table-column prop="address" label="地址"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="birth" label="出生日期"></el-table-column>
            <el-table-column prop="postcode" label="邮政编码"></el-table-column>
            <el-table-column prop="register_time" label="注册日期"></el-table-column>
            <el-table-column prop="userface" label="头像"></el-table-column>
            <el-table-column label="操作" aling="center" width="100">
                <template v-slot="slot">
                    <el-button size="mini" type = "text" @click="toBindRole(slot.row)">绑定角色</el-button>
                </template>
            </el-table-column>
        </el-table>
        <!-- 模态框 -->
         
        <el-dialog title="绑定角色" :visible.sync="visible">
            {{user}}
            <el-form >
            <el-form-item label="用户名" label-width="80px">
                <strong>{{user.name}}</strong>
            </el-form-item>
            <el-form-item label="角色" label-width="80px">
               <el-checkbox-group v-model="user.roles">
                    <el-checkbox v-for="r in roles" :key=r.id :label="r.id" >{{r.name}}</el-checkbox>    
                </el-checkbox-group>
            </el-form-item>
           
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="visible = false">取 消</el-button>
                <el-button type="primary" @click="bindRoleHandler">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
import request from '@/utils/request'
import qs from 'querystring'
export default {
    data(){
        return{
            visible:false,
            form:{},
            users:[],//所有的用户信息
            user:{},//当前用户
            roles:[],//所有的角色信息
        }
    },
    created(){
        this.loadUsers();
        this.loadRoles();
    },
    methods:{
        //加载用户信息
        loadUsers(){
            request.get("/baseUser/findAllWithRole")
            .then(response=>{
                //数据清洗,将原先roles里面是角色对象替换成角色的id
                response.data.forEach(item=>{
                    item.roles=item.roles.map(r=>r.id)
                })
                this.users=response.data;
                
            })
            
        },
        //加载所有角色
        loadRoles(){
            request.get("/role/findAll")
            .then(response=>{
                this.roles = response.data;
            })
        },
        //绑定角色
        toBindRole(user){
            this.user = user;
            this.visible=true;   
        },
        //更新用户角色
        bindRoleHandler(){
            request.request({
                url:'/baseUser/setRoles',
                method:'post',
                headers:{
                    'Content-Type':'application/x-www-form-urlencoded'
                },
                data:qs.stringify(this.user)
            })
            .then(response=>{
                this.visible=false;
                this.$message({message:response.message,type:"success"})
                this.loadUsers();
            })
        }
    }
}
</script>
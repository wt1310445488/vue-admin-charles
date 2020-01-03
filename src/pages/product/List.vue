<template>
    <div>
        <!-- 按钮 -->
        <el-button  size="small" type="primary" @click="toAddHandler">添加</el-button>
        <el-button  size="small" type="danger" >删除</el-button>
        <!-- 按钮结束 -->
        <!-- 表格 -->
        <el-table :data="products">
        <el-table-column fixed="left" prop = "id" label="编号"></el-table-column>
        <el-table-column fixed="left" prop = "name" label="产品名"></el-table-column>
        <el-table-column prop = "description" label="描述"></el-table-column>
        <el-table-column prop = "price" label="价格"></el-table-column>
        <el-table-column prop = "categoryId" label="类别"></el-table-column>
        <el-table-column fixed="right" label="操作">
            <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandler(slot.row.id)">
                    <i class="el-icon-delete"></i>
                </a>
                <a href="" @click.prevent="toUpdateHandler(slot.row.id)">
                    <i class="el-icon-edit-outline"></i>
                    </a>
                <a href="" >详情</a>

            </template>
        </el-table-column>
        </el-table>
        <!-- 表格结束 -->
         <!-- 分页 -->
        <el-pagination
           layout="prev, pager, next"
          :total="50">
          </el-pagination>
        <!-- 分页 -->
        <!-- 模态框 -->
         <el-dialog
           :title="title"
           :visible.sync="visible"
            width="60%"
          >{{form}}
          <el-form :model="form" label-width="80px">
            <el-form-item label="产品名">
              <el-input v-model="form.name">
              </el-input>
            </el-form-item>
             <el-form-item label="编号">
              <el-input  v-model="form.id">
              </el-input>
            </el-form-item>
            <el-form-item label="描述">
              <el-input v-model="form.description">
              </el-input>
            </el-form-item>
             <el-form-item label="价格">
              <el-input v-model="form.price">
              </el-input>
            </el-form-item>
             <el-form-item label="类别" >
              <el-input v-model="form.categoryId">
              </el-input>
             </el-form-item>
          </el-form>
        <span>录入产品信息</span>
        <span slot="footer" class="dialog-footer">
        <el-button size="small"  @click="closeModalHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
        </span>
        </el-dialog>
        <!-- 模态框 -->
    </div>   
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    created(){
    //在页面出来的时候加载数据
    this.loadData();
     },
    data(){
       return{
           title:"title",
           visible:false,
           products:[],
           form:{
             type:"product"
           }
           }
          },
    methods:{
     submitHandler(){
        let url = "http://localhost:6677/product/saveOrUpdate";
        //前端向后台发送请求，完成数据的保存操作
        request({  
          url,
          method:"POST",
          //告诉后台我的请求体中放的是查询字符串
          headers:{
           "Content-Type":"application/x-www-form-urlencoded"
          },
          //请求体中的数据，将this.form转换为查询字符串发送给后台
          data:querystring.stringify(this.form)
        }).then((response)=>{
          this.closeModalHandler();
          this.loadDate();
          this.$message({          
            type:"success",
            message:response.message
            })
        })
      },
      //重载产品数据
     loadData(){
        let url = "http://localhost:6677/product/findAll";
        request.get(url).then((response)=>{
          //箭头函数中的this指向外部函数中的this
          this.products = response.data;
        })
      },
     closeModalHandler(){
           this.visible=false;
       },
     toAddHandler(){
       this.title="录入产品信息";
       this.visible=true;
       },
     toDeleteHandler(id){

          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
       },
     toUpdateHandler(id){
        this.title="更新产品信息";
        this.visible=true;
       }
   }

}
</script>
<style scoped>

</style>>
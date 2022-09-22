<template>
    <el-container style="min-height: 100vh ">
<!--      侧边栏-->
      <el-aside :width=" sideWidth+'px' " style="background-color: rgb(238, 241, 246); box-shadow: 2px 0 6px rgb(21 0 41 / 35%)" >
        <el-menu :default-openeds="['1', '3']" style="min-height: 100%;overflow-x: hidden;"
                 background-color="rgb(48,65,86)"
                 text-color="#fff"
                 active-text-color="#ffd04b"
                 :collapse-transition="false"
                 :collapse="isCollapse"
        >

<!--          顶部的标签logo-->
          <div style="height: 60px;line-height: 60px;text-align: center">
            <img src="../assets/logo.png" style="width:20px;position:relative;top:5px;margin-right: 2px">
            <b style="color: white" v-show="logoTextShow">后台管理系统</b>
          </div>

          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-message"></i>
              <span>导航一</span>
            </template>
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="1-1">选项1</el-menu-item>
              <el-menu-item index="1-2">选项2</el-menu-item>
            </el-menu-item-group>
            <el-menu-item-group title="分组2">
              <el-menu-item index="1-3">选项3</el-menu-item>
            </el-menu-item-group>
            <el-submenu index="1-4">
              <template slot="title">选项4</template>
              <el-menu-item index="1-4-1">选项4-1</el-menu-item>
            </el-submenu>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-menu"></i>
              <span>导航二</span>
            </template>
            <el-menu-item-group>
              <template slot="title">分组一</template>
              <el-menu-item index="2-1">选项1</el-menu-item>
              <el-menu-item index="2-2">选项2</el-menu-item>
            </el-menu-item-group>

          </el-submenu>

        </el-menu>
      </el-aside>

      <el-container>

<!--        头部header-->
        <el-header style="font-size: 12px;border-bottom: 1px solid #ccc;line-height: 60px;display: flex; "  >

          <div style="flex: 1;font-size: 18px">
             <span :class="collapseBtnClass" style="cursor: pointer" @click="collapse"></span>
          </div>

          <el-dropdown style="width: 100px;cursor: pointer">
            <span >王小虎</span><i class="el-icon-arrow-down"  style="margin-left: 8px;color: #eeeeee"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人信息</el-dropdown-item>
              <el-dropdown-item>退出系统</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>

        </el-header>

<!--        主体-->
        <el-main>

          <el-breadcrumb separator="/">
            <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item><a href="/">活动管理</a></el-breadcrumb-item>
            <el-breadcrumb-item>活动列表</el-breadcrumb-item>
            <el-breadcrumb-item>活动详情</el-breadcrumb-item>
          </el-breadcrumb>


<!--          搜索框-->
          <div style="margin: 14px 0">
<!--            搜索框的控件-->
            <el-input style="width: 200px;" placeholder="请输入内容" suffix-icon="el-icon-search"></el-input>
            <el-input style="width: 250px;" placeholder="请输入邮箱" suffix-icon="el-icon-message"></el-input>
            <el-input style="width: 250px;" placeholder="请输入地址" suffix-icon="el-icon-position"></el-input>
            <el-button type="primary" class="ml-5" icon="el-icon-search">搜索</el-button>
          </div>

<!--          一排编辑的按钮-->
          <div style="margin: 12px 0">
<!--            新增按钮-->
            <el-button type="primary"  icon="el-icon-circle-plus-outline">新增</el-button>
<!--            批量删除-->
            <el-button type="primary"  icon="el-icon-remove-outline">批量删除</el-button>
<!--            导入-->
            <el-button type="primary"  icon="el-icon-upload2">导入</el-button>
<!--          导出-->
            <el-button type="primary"  icon="el-icon-download">导出</el-button>
          </div>

<!--           这是表单-->
          <el-table :data="tableData" border stripe  >
            <el-table-column prop="date" label="日期" width="200" align="center">
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="200" align="center">
            </el-table-column>
            <el-table-column prop="address" label="地址" width="600" align="center">
            </el-table-column>
            <el-table-column prop="edit" label="编辑" align="center">
              <template slot-scope="scope">
                <el-button type="success" icon="el-icon-edit" >修改</el-button>
                <el-button type="danger" icon="el-icon-delete">删除</el-button>
              </template>
            </el-table-column>
          </el-table>

<!--      这是分页的组件-->
          <div class="block" style="padding: 12px 0;margin-left: 250px">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="currentPage4"
              :page-sizes="[5, 10, 15, 20]"
              :page-size="10"
              layout="total, sizes, prev, pager, next, jumper"
              :total="400">
            </el-pagination>
          </div>

        </el-main>

      </el-container>
    </el-container>

</template>

<script>
export default {
  name: "Home",
  data() {
    const item = {
      date: '2016-05-02',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1518 弄',
    };
    return {
      tableData: Array(10).fill(item),
      collapseBtnClass:'el-icon-s-fold',
      isCollapse:false,
      sideWidth:200,
      logoTextShow:true
    }
  },methods:{
    collapse(){ // 点击收缩按钮收缩
        this.isCollapse=!this.isCollapse;
        if(this.isCollapse){
          this.sideWidth = 64;
          this.collapseBtnClass='el-icon-s-unfold';
          this.logoTextShow=false;
        }else{
          this.sideWidth=200;
          this.collapseBtnClass='el-icon-s-fold';
          this.logoTextShow=true;
        }
    }
  }
}
</script>

<style>
.el-header {
  color: black;
  line-height: 60px;
}

.el-aside {
  color: #333;
}


</style>

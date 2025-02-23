<template>
  <div class="manager-container">
    <div class="manager-header">
      <div class="manager-header-left">
        <img src="@/assets/imgs/logo.png" alt="">
        <div class="title">校园快递代取平台</div>
      </div>
      <div class="manager-header-center">
        <el-breadcrumb separator="/">
          <el-breadcrumb-item :to="{ path: '/manager/home' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item>{{ router.currentRoute.value.meta.name }}</el-breadcrumb-item>
        </el-breadcrumb>
      </div>
      <div class="manager-header-right">
        <el-dropdown style="cursor: pointer">
          <div style="padding-right: 20px; display: flex; align-items: center">
            <img style="width: 40px; height: 40px; border-radius: 50%;" :src="data.user.avatar" alt="">
            <span style="margin-left: 5px; color: white">{{ data.user.name }}</span><el-icon color="#fff"><arrow-down /></el-icon>
          </div>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item @click="router.push('/manager/person')">个人资料</el-dropdown-item>
              <el-dropdown-item @click="router.push('/manager/password')">修改密码</el-dropdown-item>
              <el-dropdown-item @click="logout">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
    </div>
    <!-- 下面部分开始 -->
    <div style="display: flex">
      <div class="manager-main-left">
        <el-menu :default-active="router.currentRoute.value.path"
                 :default-openeds="['1', '2', '3', '4','5']"
        router
        >
        <!-- 系统首页菜单项 -->
        <el-menu-item index="/manager/home">
          <el-icon><HomeFilled /></el-icon>
          <span>系统首页</span>
        </el-menu-item>

          <!-- 用户管理菜单 -->
          <el-sub-menu index="1">
            <template #title>
              <el-icon><Menu /></el-icon>
              <span>用户管理</span>
            </template>
            <el-menu-item index="/manager/student">学生信息</el-menu-item>
            <el-menu-item index="/manager/courier">代取员信息</el-menu-item>
            <el-menu-item index="/manager/admin">管理员信息</el-menu-item>
          </el-sub-menu>

        <!-- 信息管理菜单 -->
        <el-sub-menu index="2">
          <template #title>
            <el-icon><Menu /></el-icon>
            <span>信息管理</span>
          </template>
          <el-menu-item index="/manager/orders">代取订单信息</el-menu-item>
          <el-menu-item index="/manager/expressType">代取快递类型</el-menu-item>
          <el-menu-item index="/manager/identification">代取员认证信息</el-menu-item>
          <el-menu-item index="/manager/courierCommission">代取员等级信息</el-menu-item>
          <el-menu-item index="/manager/upgradeApply">代取员等级提升申请</el-menu-item>
          <el-menu-item index="/manager/cashRecord">代取员提现申请</el-menu-item>
          <el-menu-item index="/manager/releaseApply">代取员解封申请</el-menu-item>
          <el-menu-item index="/manager/address">学生地址薄</el-menu-item>
          <el-menu-item index="/manager/information">资讯信息</el-menu-item>
          <el-menu-item index="/manager/feedback">反馈信息</el-menu-item>
          <el-menu-item index="/manager/notice">系统公告</el-menu-item>
        </el-sub-menu>

        <!-- 货物管理菜单 -->
        <el-sub-menu index="3">
          <template #title>
            <el-icon><Menu /></el-icon>
            <span>货物管理</span>
          </template>
          <el-menu-item index="/manager/goods">货物信息</el-menu-item>
          <el-menu-item index="/manager/inventory">库存管理</el-menu-item>
          <el-menu-item index="/manager/shipments">货物发货</el-menu-item>
        </el-sub-menu>

        <!-- 车辆管理菜单 -->
        <el-sub-menu index="4">
          <template #title>
            <el-icon><Menu /></el-icon>
            <span>车辆管理</span>
          </template>
          <el-menu-item index="/manager/vehicles">车辆信息</el-menu-item>
          <el-menu-item index="/manager/vehicleMaintenance">车辆维护</el-menu-item>
          <el-menu-item index="/manager/vehicleAssignment">车辆调度</el-menu-item>
        </el-sub-menu>

          <!-- 系统日志信息菜单 -->
          <el-sub-menu index="5">
            <template #title>
              <el-icon><Menu /></el-icon>
              <span>系统日志</span>
            </template>
            <el-menu-item index="/manager/systemLogs">日志信息</el-menu-item>
            <el-menu-item index="/manager/errorLogs">错误日志</el-menu-item>
            <el-menu-item index="/manager/userActivityLogs">用户操作日志</el-menu-item>
          </el-sub-menu>
        </el-menu>
      </div>

      <div class="manager-main-right">
        <RouterView @updateUser="updateUser" />
      </div>
    </div>
    <!-- 下面部分结束 -->


  </div>
</template>

<script setup>
import { reactive } from "vue";
import router from "@/router/index.js";
import {ElMessage} from "element-plus";

const data = reactive({
  user: JSON.parse(localStorage.getItem('xm-user') || '{}')
})

if (data.user.role !== 'ADMIN') {
  location.href = '/login'
}

const logout = () => {
  localStorage.removeItem('xm-user')
  router.push('/login')
}

const updateUser = () => {
  data.user =  JSON.parse(localStorage.getItem('xm-user') || '{}')
}

if (!data.user.id) {
  logout()
  ElMessage.error('请登录！')
}
</script>

<style scoped>
@import "@/assets/css/manager.css";
</style>
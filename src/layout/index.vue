<template>
  <div class="layout-container">
    <!-- 左侧菜单 -->
    <div
      class="layout-slider"
      :class="{ fold: LayOutSettingStore.fold ? true : false }"
    >
      <Logo :fold="LayOutSettingStore.fold"></Logo>
      <!-- 展示菜单 -->
      <!-- 滚动组件 -->
      <el-scrollbar class="scrollbar">
        <!-- 菜单组件 -->
        <el-menu
          :collapse="LayOutSettingStore.fold"
          :default-active="$route.path"
          background-color="#001529"
          text-color="white"
          active-text-color="#19CAAD"
          style="border-right: none"
        >
          <!--根据路由动态生成菜单-->
          <Menu :menuList="userStore.menuRoutes"></Menu>
        </el-menu>
      </el-scrollbar>
    </div>

    <!-- 顶部导航 -->
    <div
      class="layout-tabbar"
      :class="{ fold: LayOutSettingStore.fold ? true : false }"
    >
      <Tabbar></Tabbar>
    </div>

    <!-- 内容展示区域 -->
    <div
      class="layout-main"
      :class="{ fold: LayOutSettingStore.fold ? true : false }"
    >
      <Main></Main>
    </div>
  </div>
</template>

<script setup lang="ts" name="Layout">
// 菜单栏logo区
import Logo from './logo/index.vue'
// 菜单栏
import Menu from './menu/index.vue'
// 右侧导航栏
import Tabbar from './tabbar/index.vue'
// 右侧内容展示区
import Main from './main/index.vue'
// 获取用户相关小仓库
import useUserStore from '@/store/modules/user'
// 获取layout配置相关仓库
import useLayOutSettingStore from '@/store/modules/setting'
// 获取路由对象
import { useRoute } from 'vue-router'

let userStore = useUserStore()
let $route = useRoute()

let LayOutSettingStore = useLayOutSettingStore()
</script>

<style scoped lang="scss">
.layout-container {
  width: 100%;
  height: 100vh;

  .layout-slider {
    color: white;
    width: $base-menu-width;
    height: 100vh;
    background-color: $base-menu-background;
    transition: all 0.3s;

    .scrollbar {
      width: 100%;
      height: calc(100vh - $base-menu-logo-height);
    }

    &.fold {
      width: $base-menu-min-width;
    }
  }

  .layout-tabbar {
    width: calc(100% - $base-menu-width);
    height: $base-tabbar-height;
    position: fixed;
    top: 0;
    left: $base-menu-width;
    transition: all 0.3s;

    &.fold {
      width: calc(100vw - $base-menu-min-width);
      left: $base-menu-min-width;
    }
  }

  .layout-main {
    position: absolute;
    left: $base-menu-width;
    top: $base-tabbar-height;
    width: calc(100% - $base-menu-width);
    height: calc(100vh - $base-tabbar-height);
    padding: 20px;
    overflow: auto;
    transition: all 0.3s;

    &.fold {
      width: calc(100vw - $base-menu-min-width);
      left: $base-menu-min-width;
    }
  }
}
</style>

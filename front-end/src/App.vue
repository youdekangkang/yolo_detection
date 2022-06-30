<template>
  <div id="app">
    <el-container>
      <el-header style="background-color: #03033f; color: #b8bbcc; height: 100px">
        <app-header></app-header>
      </el-header>
      <el-container>
        <el-aside width="300px" style="background-color: white; height: 100vh">
          <el-menu
              :default-active="defaultRouter"
              class="el-menu-vertical-demo"
              @select="handleSelect"
              style="margin-top: 50px"
              text-color="black"
              active-text-color="#5245e5"
          >
            <el-menu-item index="f-1">
              <i class="el-icon-menu"></i>
              <span slot="title">图像检测 - 功能演示</span>
            </el-menu-item>
            <el-menu-item index="f-2">
              <i class="el-icon-menu"></i>
              <span slot="title">视频检测 - 结果展示</span>
            </el-menu-item>
          </el-menu>
        </el-aside>
        <el-container>
          <el-main>
            <keep-alive>
              <app-1 :is="currentApp"></app-1>
            </keep-alive>
          </el-main>
        </el-container>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import App1 from "./components/App1";
import App2 from "./components/App2";

const defaultRouter = "f-1";
const routerMap = {
  "f-1": "app-1",
  "f-2": "app-2"
}


export default {
  name: "main",
  data() {
    return {
      defaultRouter: defaultRouter,
      currentApp: routerMap[defaultRouter],
    };
  },
  components: {
    "app-header": Header,
    "app-footer": Footer,
    "app-1": App1,
    "app-2": App2,
  },
  methods: {
    handleSelect(key) {
      this.currentApp = routerMap[key]
    }
  },
};
</script>

<style>
</style>

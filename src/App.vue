<!--suppress ALL -->
<template>
  <div class="qwq-main">
    <el-tabs class="qwq-tabs" v-model="activeName" type="border-card" @tab-click="handleClick">
      <el-tab-pane label="状态" name="status">
        <el-descriptions
            title="QwQ状态"
            direction="horizontal"
            :column="1"
            border>
          <el-descriptions-item label="运行模式">
            <el-tag >{{status}}</el-tag>
          </el-descriptions-item>
          <el-descriptions-item label="应用版本" > {{version}} </el-descriptions-item>
          <el-descriptions-item label="模块版本">{{moduleVersion}}</el-descriptions-item>
        </el-descriptions>

        <el-scrollbar >

        </el-scrollbar>

      </el-tab-pane>
      <el-tab-pane label="聊天设置" name="chat"><ChatSetting /></el-tab-pane>
      <el-tab-pane label="消息设置" name="message"><MessageSetting /></el-tab-pane>
      <el-tab-pane label="群聊设置" name="group"><GroupSetting /></el-tab-pane>
      <el-tab-pane label="个人设置" name="me"><SettingMe /></el-tab-pane>
      <el-tab-pane label="实验室" name="lab"><LabSetting /></el-tab-pane>
    </el-tabs>
  </div>
</template>

<script setup lang="ts">
import ChatSetting from "@/components/layouts/ChatSetting.vue";
import SettingMe from "@/components/layouts/SettingMe.vue";
import GroupSetting from "@/components/layouts/GroupSetting.vue";
import MessageSetting from "@/components/layouts/MessageSetting.vue";
import LabSetting from "@/components/layouts/LabSetting.vue";
import { ref, reactive, toRefs, watch } from 'vue'
import type { TabsPaneContext } from 'element-plus'

const activeName = ref('status')
const handleClick = (tab: TabsPaneContext, event: Event) => {
  //console.log(tab, event)
}

const count = ref(111)
const load = () => {
  count.value += 2
}

const loading = ref(true)
const status = ref("unknown");
const version = ref("unknown");
const moduleVersion = ref("unknown");

if (typeof qwq === "undefined") {
  //window.location.href = "https://im.qq.com";
} else {
  qwq.toast('QwQ模块载入成功')
  loading.value = false
  version.value = qwq.getQQVersion()
  status.value = qwq.getStatus()
  moduleVersion.value = qwq.getModuleVersion()
}

</script>

<style scoped>
@import "@/assets/qwq.css";

.qwq-main {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.qwq-tabs {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.el-tabs__content {
  flex: 1;
  overflow: auto;
}


</style>

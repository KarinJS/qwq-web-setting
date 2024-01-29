<!--suppress ALL -->
<template>
  <div class="center-vertical">
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

    <el-collapse v-model="activeName" accordion>
      <ChatSetting/>
      <SettingMe />
      <GroupSetting />
      <LabSetting />
    </el-collapse>
  </div>
</template>

<script setup lang="ts">
import ChatSetting from "@/components/layouts/ChatSetting.vue";
import SettingMe from "@/components/layouts/SettingMe.vue";
import GroupSetting from "@/components/layouts/GroupSetting.vue";
import LabSetting from "@/components/layouts/LabSetting.vue";
import { ref, reactive, toRefs, watch } from 'vue'

const activeName = ref('')
const loading = ref(true)
const status = ref("unknown");
const version = ref("unknown");
const moduleVersion = ref("unknown");

if (typeof qwq === "undefined") {
  // alert('QwQ bridge is not loaded')
  window.location.href = "https://im.qq.com";
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

.center-vertical {
  position: relative;
}
</style>

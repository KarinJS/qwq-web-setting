<script setup>
import {ref} from "vue";

const status = ref("unknown");
const version = ref("unknown");
const moduleVersion = ref("unknown");
const loading = ref(true)

if (typeof qwq === "undefined") {
  window.location.href = "https://im.qq.com";
} else {
  qwq.toast('QwQ模块载入成功')
  loading.value = false
  version.value = qwq.getQQVersion()
  status.value = qwq.getStatus()
  moduleVersion.value = qwq.getModuleVersion()
}
</script>

<template>
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
</template>

<style scoped>

</style>
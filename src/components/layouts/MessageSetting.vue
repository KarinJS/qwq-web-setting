<!--suppress ALL -->

<template>
  <div class="settings-item-first">
    <div class="settings-text">小尾巴</div>
    <el-row class="nickname-item" @click="showDialog = true">
      <el-col :span="20">
        <span>QwQ</span>
      </el-col>
      <el-col :span="4" class="arrow">
        <el-icon><arrow-right /></el-icon>
      </el-col>
    </el-row>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'
import { ArrowRight } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus';

const showDialog = ref(false)

/**
 * 消息设置
 */
const messageTail = ref(false)

let settingMap = new Map([
  ["message_tail", messageTail]
])

let settingDisableMap = reactive(new Map([
  ["message_tail", false],
]))

if (typeof qwq !== "undefined") {
  for (let [key, ref] of settingMap) {
    let settingJsonStr = qwq.getSetting(key)
    let setting = JSON.parse(settingJsonStr)
    let failed = setting.failed
    let value = setting.value

    settingDisableMap.set(key, failed)
    if(value === undefined) {
      value = false
    } else {
      ref.value = value
    }
    watch(ref, async (newValue) => {
      qwq.setSetting(key, newValue)
    })
  }
}
</script>

<style scoped>
.nickname-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  cursor: pointer;
}

.arrow {
  text-align: right;
}

.el-form {
  max-width: 400px;
}
</style>
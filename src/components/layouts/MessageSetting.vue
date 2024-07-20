<!--suppress ALL -->

<template>
  <el-container class="settings-container">
    <el-main>

      <div class="settings-item-first">
        <div class="settings-text">小尾巴</div>
        <el-form :model="form" ref="form">
          <el-form-item>
            <el-col :span="18">
              <el-input v-model="form.settingValue" placeholder="Enter your setting"></el-input>
            </el-col>
            <el-col :span="6">
              <el-button type="primary" @click="saveSetting">Save</el-button>
            </el-col>
          </el-form-item>
        </el-form>
      </div>

    </el-main>
  </el-container>
</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'
import { ElMessage } from 'element-plus';

const form = ref({
  settingValue: '',
});

const saveSetting = () => {
  ElMessage({
    message: `Setting saved: ${form.value.settingValue}`,
    type: 'success',
  });
  // 这里可以添加保存设置的逻辑，例如发送请求到服务器
};

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

<style>
@import "@/assets/qwq.css";
</style>


<style scoped>
.el-form {
  max-width: 400px;
}
</style>
<!--suppress ALL -->

<template>
  <div class="settings-item">
    <div class="settings-text">禁止使用流量热更新SO文件</div>
    <el-switch v-model="disableHotUpdateSoByTraffic" :disabled="settingDisableMap.get('disable_hot_update_so_by_traffic')" />
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">拦截QQ无用发包</div>
        <el-switch v-model="disableUselessPacket" :disabled="settingDisableMap.get('disable_useless_packet')" />
      </el-row>
      <div class="settings-subtext">
        禁止QQ发送没什么鸟用的网络包占用网络资源，同时该功能会的开启会自动开启主题修复。
      </div>
    </el-col>
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">禁用QQ崩溃上报</div>
        <el-switch v-model="disableQQCrashReport" :disabled="settingDisableMap.get('disable_qq_crash_report')" />
      </el-row>
      <div class="settings-subtext">
        禁止QQ崩溃上报日志以及禁用崩溃后自杀（阻止闪退?）。
      </div>
    </el-col>
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">禁用扫码授权检查</div>
        <el-switch v-model="disableQRLoginCheck" :disabled="settingDisableMap.get('disable_qrlogin_check')" />
      </el-row>
      <div class="settings-subtext">
        禁用后，使用相册或长按识别二维码时不再阻止扫码授权登录。
      </div>
    </el-col>
  </div>

</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'
import {ElMessage} from "element-plus";

/**
 * 实验功能
 */
const disableHotUpdateSoByTraffic = ref(false)
const disableUselessPacket = ref(false)
const disableQQCrashReport = ref(false)
const disableQRLoginCheck = ref(false)

let settingMap = new Map([
  ["disable_hot_update_so_by_traffic", disableHotUpdateSoByTraffic],
  ["disable_useless_packet", disableUselessPacket],
  ["disable_qq_crash_report", disableQQCrashReport],
  ["disable_qrlogin_check", disableQRLoginCheck],
])
let settingDisableMap = reactive(new Map([
  ["disable_hot_update_so_by_traffic", false],
  ["disable_useless_packet", false],
  ["disable_qq_crash_report", false],
  ["disable_qrlogin_check", false],
]))

if (typeof qwq !== "undefined") {
  for (let [key, ref] of settingMap) {
    let settingJsonStr = qwq.getSetting(key)
    let setting = JSON.parse(settingJsonStr)
    let failed = setting.failed
    let value = setting.value

    settingDisableMap.set(key, failed)
    ref.value = value
    watch(ref, async (newValue) => {
      ElMessage({
        message: `重启QQ生效`,
        type: 'success',
      });
      qwq.setSetting(key, newValue)
    })
  }
}
</script>

<style>
@import "@/assets/qwq.css";
</style>
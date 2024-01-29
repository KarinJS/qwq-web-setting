<!--suppress ALL -->

<template>
  <el-collapse-item title="实验功能" name="lab">
    <div class="settings-item-first">
      <div class="settings-text">禁止使用流量热更新SO文件</div>
      <el-switch v-model="disableHotUpdateSoByTraffic" :disabled="settingDisableMap.get('disable_hot_update_so_by_traffic')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">拦截QQ无用发包</div>
      <el-switch v-model="disableUselessPacket" :disabled="settingDisableMap.get('disable_useless_packet')" />
    </div>

    <div class="settings-subtext">
      禁止QQ发送没什么鸟用的网络包占用网络资源，同时该功能会的开启会自动开启主题修复。
    </div>
  </el-collapse-item>
</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'

/**
 * 实验功能
 */
const disableHotUpdateSoByTraffic = ref(false)
const disableUselessPacket = ref(false)

let settingMap = new Map([
  ["disable_hot_update_so_by_traffic", disableHotUpdateSoByTraffic],
  ["disable_useless_packet", disableUselessPacket],
])
let settingDisableMap = reactive(new Map([
  ["disable_hot_update_so_by_traffic", false],
  ["disable_useless_packet", false],
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
      qwq.setSetting(key, newValue)
    })
  }
}
</script>

<style>
@import "@/assets/qwq.css";
</style>
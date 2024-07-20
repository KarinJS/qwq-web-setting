<!--suppress ALL -->

<template>
  <el-container class="settings-container">
    <el-main>
      <div class="settings-item-first">
        <div class="settings-text">主页侧边栏简化</div>
        <el-switch v-model="simplifyHomepageSidebar" :disabled="settingDisableMap.get('simplify_homepage_sidebar')" />
      </div>

      <div class="settings-item">
        <div class="settings-text">禁止自动更新检查</div>
        <el-switch v-model="disableUpdateCheck" :disabled="settingDisableMap.get('disable_update_check')" />
      </div>
      <!--
              <div class="settings-subtext">
                禁止QQ获取更新消息，检查当前版本是否为最新版本。
              </div>-->

      <div class="settings-item">
        <div class="settings-text">一键点赞20次</div>
        <el-switch v-model="oneClickLike" :disabled="settingDisableMap.get('one_click_like')" />
      </div>

      <div class="settings-item">
        <div class="settings-text">强制平板模式登录</div>
        <el-switch v-model="forceTabletMode" :disabled="settingDisableMap.get('force_tablet_mode')" />
      </div>

      <!--        <div class="settings-subtext">
                实现两台安卓手机设备共存登录，解放生产力。
              </div>-->
    </el-main>
  </el-container>
</template>

<script setup lang="ts">
import { ref, reactive, watch } from 'vue'

/**
 * 个人设置
 */
const simplifyHomepageSidebar = ref(false)
const disableUpdateCheck = ref(false)
const oneClickLike = ref(false)
const forceTabletMode = ref(false)

let settingMap = new Map([
  ["simplify_homepage_sidebar", simplifyHomepageSidebar],
  ["disable_update_check", disableUpdateCheck],
  ["one_click_like", oneClickLike],
  ["force_tablet_mode", forceTabletMode],
])
let settingDisableMap = reactive(new Map([
  ["simplify_homepage_sidebar", false],
  ["disable_update_check", false],
  ["one_click_like", false],
  ["force_tablet_mode", false],
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
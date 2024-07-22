<!--suppress ALL -->

<template>
  <div class="settings-item">
    <div class="settings-text">消息反撤回</div>
    <el-switch v-model="interceptRecall" :disabled="settingDisableMap.get('intercept_recall')" />
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">简化气泡字体</div>
        <el-switch v-model="simplifyBubbleFont" :disabled="settingDisableMap.get('simplify_bubble_font')" />
      </el-row>
      <div class="settings-subtext">让气泡和字体变成默认的，避免阴间气泡和阴间字体叠加使得文字难以看懂。</div>
    </el-col>
  </div>

  <div class="settings-item">
    <div class="settings-text">简洁圆头像</div>
    <el-switch v-model="simplifyBubbleAvatar" :disabled="settingDisableMap.get('simplify_bubble_avatar')" />
  </div>

  <div class="settings-item">
    <div class="settings-text">以图片的方式打开闪照</div>
    <el-switch v-model="disableFlashPicture" :disabled="settingDisableMap.get('disable_flash_picture')" />
  </div>

  <div class="settings-item">
    <div class="settings-text">允许群聊闪照消息</div>
    <el-switch v-model="allowGroupFlashPic" :disabled="settingDisableMap.get('allow_group_flash_pic')" />
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">复读机 +1</div>
        <el-switch v-model="repeatMessage" :disabled="settingDisableMap.get('repeat_message')" />
      </el-row>
      <div class="settings-subtext">让气泡和字体变成默认的，避免阴间气泡和阴间字体叠加使得文字难以看懂。</div>
    </el-col>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'
import {ElMessage} from "element-plus";

/**
 * 聊天设置
 */
const interceptRecall = ref(false)
const simplifyBubbleFont = ref(false)
const simplifyBubbleAvatar = ref(false)
const repeatMessage = ref(false)
const disableFlashPicture = ref(false)
const allowGroupFlashPic = ref(false)

let settingMap = new Map([
  ["intercept_recall", interceptRecall],
  ["simplify_bubble_font", simplifyBubbleFont],
  ["simplify_bubble_avatar", simplifyBubbleAvatar],
  ["repeat_message", repeatMessage],
  ["disable_flash_picture", disableFlashPicture],
  ["allow_group_flash_pic", allowGroupFlashPic],
])

let settingDisableMap = reactive(new Map([
  ["intercept_recall", false],
  ["simplify_bubble_font", false],
  ["simplify_bubble_avatar", false],
  ["repeat_message", false],
  ["disable_flash_picture", false],
  ["allow_group_flash_pic", false],
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

<style scoped>
</style>
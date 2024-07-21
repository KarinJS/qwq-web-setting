<!--suppress ALL -->

<template>
  <div class="settings-item">
    <div class="settings-text">屏蔽入群动画</div>
    <el-switch v-model="disableVisitGroupAnimation" :disabled="settingDisableMap.get('disable_visit_group_animation')" />
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">群文件增强</div>
        <el-switch v-model="superGroupFile" :disabled="settingDisableMap.get('super_group_file')" />
      </el-row>
      <div class="settings-subtext">
        允许自定义文件夹或文件名称，挪动文件到指定文件夹，上传应用自动重命名，下载应用去除<el-text class="mx-1" type="primary">.1</el-text>后缀。
      </div>
    </el-col>
  </div>

  <div class="settings-item">
    <div class="settings-text">显示群禁言操作者</div>
    <el-switch v-model="showBanOperator" :disabled="settingDisableMap.get('show_ban_operator')" />
  </div>

  <div class="settings-item">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">艾特群成员列表排序优化</div>
        <el-switch v-model="optimizeAtSort" :disabled="settingDisableMap.get('optimize_at_sort')" />
      </el-row>
      <div class="settings-subtext">
        使得艾特群成员时，群成员列表按照群主/管理员优选的方式排序。
      </div>
    </el-col>
  </div>

</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'
import {ElMessage} from "element-plus";

/**
 * 群聊设置
 */
const disableVisitGroupAnimation = ref(false)
const superGroupFile = ref(false)
const showBanOperator = ref(false)
const optimizeAtSort = ref(false)

let settingMap = new Map([
  ["disable_visit_group_animation", disableVisitGroupAnimation],
  ["super_group_file", superGroupFile],
  ["show_ban_operator", showBanOperator],
  ["optimize_at_sort", optimizeAtSort]
])

let settingDisableMap = reactive(new Map([
  ["disable_visit_group_animation", false],
  ["super_group_file", false],
  ["show_ban_operator", false],
  ["optimize_at_sort", false]
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
        message: `修改成功，重启QQ生效`,
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
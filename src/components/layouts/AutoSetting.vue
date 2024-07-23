<script setup>
import { ref, reactive, toRefs, watch } from 'vue'
import {ElMessage} from "element-plus";

// 群自动签到
const autoGroupSign = ref(false)
// 群自动续火
const autoKeepGroupFire = ref(false)
// 私聊自动续火
const autoKeepC2CFire = ref(false)
// 自动收集私聊奇异能量
const autoCollectEnergy = ref(false)

// 自动抽卡群幸运字符
const autoGroupLuckyChar = ref(false)
// 自动抽卡私聊幸运字符
const autoC2CLuckyChar = ref(false)
// 自动主页回赞
const autoHomePageLike = ref(false)
// 自动空间点赞
const autoZoneLike = ref(false)
// 自动空间留言
const autoZoneMessage = ref(false)



let settingMap = new Map([
  ["auto_group_sign", autoGroupSign],
  ["auto_keep_group_fire", autoKeepGroupFire],
  ["auto_keep_c2c_fire", autoKeepC2CFire],
  ["auto_collect_energy", autoCollectEnergy],
  ["auto_group_lucky_char", autoGroupLuckyChar],
  ["auto_c2c_lucky_char", autoC2CLuckyChar],
  ["auto_home_page_like", autoHomePageLike],
  ["auto_zone_like", autoZoneLike],
  ["auto_zone_message", autoZoneMessage],
])

let settingDisableMap = reactive(new Map([
  ["auto_group_sign", false],
  ["auto_keep_group_fire", false],
  ["auto_keep_c2c_fire", false],
  ["auto_collect_energy", false],
  ["auto_group_lucky_char", false],
  ["auto_c2c_lucky_char", false],
  ["auto_home_page_like", false],
  ["auto_zone_like", false],
  ["auto_zone_message", false],
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

<template>
  <div>
    <div class="settings-item">
      <div class="settings-text">群自动签到</div>
      <el-switch v-model="autoGroupSign" :disabled="settingDisableMap.get('auto_group_sign')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">群自动续火</div>
      <el-switch v-model="autoKeepGroupFire" :disabled="settingDisableMap.get('auto_keep_group_fire')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">私聊自动续火</div>
      <el-switch v-model="autoKeepC2CFire" :disabled="settingDisableMap.get('auto_keep_c2c_fire')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">自动收集私聊奇异能量</div>
      <el-switch v-model="autoCollectEnergy" :disabled="settingDisableMap.get('auto_collect_energy')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">自动抽卡群幸运字符</div>
      <el-switch v-model="autoGroupLuckyChar" :disabled="settingDisableMap.get('auto_group_lucky_char')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">自动抽卡私聊幸运字符</div>
      <el-switch v-model="autoC2CLuckyChar" :disabled="settingDisableMap.get('auto_c2c_lucky_char')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">自动主页回赞</div>
      <el-switch v-model="autoHomePageLike" :disabled="settingDisableMap.get('auto_home_page_like')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">自动空间点赞</div>
      <el-switch v-model="autoZoneLike" :disabled="settingDisableMap.get('auto_zone_like')" />
    </div>

    <div class="settings-item">
      <div class="settings-text">自动空间留言</div>
      <el-switch v-model="autoZoneMessage" :disabled="settingDisableMap.get('auto_zone_message')" />
    </div>
  </div>
</template>

<style scoped>

</style>
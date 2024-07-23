<script setup>
import {ArrowRight} from "@element-plus/icons-vue";
import {reactive, ref, watch} from "vue";
import {ElMessage} from "element-plus";

const showTailDialog = ref(false)

const saveMessageTail = () => {
  showTailDialog.value = false
  ElMessage({
    message: '重启QQ生效',
    type: 'success'
  })
  messageEncryptKey.value = messageKeyTmp.value
}


/**
 * 消息设置
 */
const simpleMessageKey = ref('')
const messageEncryptKey = ref('')
const messageKeyTmp = ref('')

watch(messageEncryptKey, (newValue, _) => {
  if (newValue.length > 0) {
    simpleMessageKey.value = "已设置"
  } else {
    simpleMessageKey.value = ""
  }
})

let settingMap = new Map([
  ["message_encrypt", messageEncryptKey]
])

let settingDisableMap = reactive(new Map([
  ["message_encrypt", false],
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
      if (typeof newValue === 'string') {
        qwq.setSettingString(key, newValue)
      } else {
        qwq.setSetting(key, newValue)
      }
    })
  }
}
</script>

<template>
  <div class="settings-item" @click="messageKeyTmp = messageEncryptKey; showTailDialog = true">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">消息加密密钥</div>

        <el-row class="text-item">
          <el-col :span="20">
            <span>{{ simpleMessageKey }}</span>
          </el-col>
          <el-col :span="4" class="arrow">
            <el-icon><arrow-right /></el-icon>
          </el-col>
        </el-row>
      </el-row>

      <div class="settings-subtext">
        消息将会通过此密钥加密抄送，保证消息安全性。
      </div>
    </el-col>
  </div>

  <el-dialog
      v-model="showTailDialog"
      title="设置您的消息加密密钥"
      :fullscreen="true"
      :modal="false"
      :append-to-body="true"
      destroy-on-close
      center
  >
    <span>
      设置消息加密，使得您的消息加密发送，当他人没有解密密钥的时候，无法正常接收解析。
    </span>
    <div>
      <strong>本功能仅提供学习与交流，禁止非法传播！</strong>
    </div>

    <el-input
        v-model="messageKeyTmp"
        style="padding-top: 10px;"
        :autosize="{ minRows: 3 }"
        type="textarea"
        placeholder="这里输入您的消息加密密钥"
    />

    <template #footer>
      <div class="dialog-footer">
        <el-button @click="showTailDialog = false">取消</el-button>
        <el-button type="primary" @click="saveMessageTail()">保存</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<style scoped>
.text-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}

.arrow {
  text-align: right;
}
</style>
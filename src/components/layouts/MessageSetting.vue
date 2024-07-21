<!--suppress ALL -->

<template>
  <div class="settings-item-first" @click="messageTailTmp = messageTailFully; showTailDialog = true">
    <el-col>
      <el-row class="settings-row">
        <div class="settings-text">小尾巴</div>

        <el-row class="nickname-item">
          <el-col :span="20">
            <span>{{ messageTail }}</span>
          </el-col>
          <el-col :span="4" class="arrow">
            <el-icon><arrow-right /></el-icon>
          </el-col>
        </el-row>
      </el-row>

      <div class="settings-subtext">
        每条消息后面携带一个小尾巴，<el-text class="mx-1" type="primary">留空则为关闭小尾巴。</el-text>
      </div>
    </el-col>
  </div>

  <el-dialog
      v-model="showTailDialog"
      title="设置您的小尾巴"
      :fullscreen="true"
      :modal="false"
      :append-to-body="true"
      destroy-on-close
      center
  >
    <span>
      设置小尾巴，小尾巴将会在您每次发消息后自动插入到消息末尾，该操作可能会导致风控。(只支持纯文本)
    </span>
    <div>
      <strong>禁止使用小尾巴传播淫秽/色情/暴力/博彩等非法内容</strong>
    </div>

    <el-input
        v-model="messageTailTmp"
        style="padding-top: 10px;"
        :autosize="{ minRows: 2 }"
        type="textarea"
        placeholder="这里输入您的小尾巴"
    />

    <template #footer>
      <div class="dialog-footer">
        <el-button @click="showTailDialog = false">取消</el-button>
        <el-button type="primary" @click="saveMessageTail()">保存</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'
import { ArrowRight } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus';

const showTailDialog = ref(false)

const saveMessageTail = () => {
  showTailDialog.value = false
  ElMessage({
    message: '修改成功',
    type: 'success'
  })
  messageTailFully.value = messageTailTmp.value
}


/**
 * 消息设置
 */
const messageTail = ref('')
const messageTailFully = ref('')
const messageTailTmp = ref('')

watch(messageTailFully, (newValue, _) => {
  if (newValue.length > 12) {
    messageTail.value = newValue.slice(0, 12)
  } else {
    messageTail.value = newValue
  }
})

let settingMap = new Map([
  ["message_tail", messageTailFully]
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
      if (typeof newValue === 'string') {
        qwq.setSettingString(key, newValue)
      } else {
        qwq.setSetting(key, newValue)
      }
    })
  }
}
</script>

<style scoped>
.nickname-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}

.arrow {
  text-align: right;
}

.el-form {
  max-width: 400px;
}
</style>
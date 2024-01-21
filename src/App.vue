<script setup>

</script>

<template>
  <div class="center-vertical">
    <el-scrollbar>
      <el-descriptions
          title="QwQ状态"
          direction="horizontal"
          :column="1"
          border>
        <el-descriptions-item label="运行模式">
          <el-tag >{{status}}</el-tag>
        </el-descriptions-item>

        <el-descriptions-item label="应用版本" > {{version}} </el-descriptions-item>
        <el-descriptions-item label="模块版本">{{moduleVersion}}</el-descriptions-item>
      </el-descriptions>

      <el-collapse v-model="activeName" accordion>
        <el-collapse-item title="聊天设置" name="chat">

          <el-container class="settings-container">
            <el-main>
              <div class="settings-item-first">
                <div class="settings-text">消息反撤回</div>
                <el-switch v-model="interceptRecall" />
              </div>
              <div class="settings-item">
                <div class="settings-text">简化气泡字体</div>
                <el-switch disabled />
              </div>

              <div class="settings-subtext">
                让气泡和字体变成默认的，避免阴间气泡和阴间字体叠加使得文字难以看懂。
              </div>

              <div class="settings-item">
                <div class="settings-text">简洁圆头像</div>
                <el-switch disabled />
              </div>

              <div class="settings-item">
                <div class="settings-text">屏蔽入群动画</div>
                <el-switch disabled />
              </div>

              <div class="settings-item">
                <div class="settings-text">群文件增强</div>
                <el-switch disabled />
              </div>

              <div class="settings-subtext">
                允许自定义文件夹或文件名称，挪动文件到指定文件夹，上传应用自动重命名，下载应用去除<el-text class="mx-1" type="primary">.1</el-text>后缀。
              </div>


              <div class="settings-item">
                <div class="settings-text">显示群禁言操作者</div>
                <el-switch disabled />
              </div>

              <div class="settings-item">
                <div class="settings-text">艾特群成员列表排序优化</div>
                <el-switch disabled />
              </div>

              <div class="settings-subtext">
                使得艾特群成员时，群成员列表按照群主/管理员优选的方式排序。
              </div>
            </el-main>
          </el-container>
        </el-collapse-item>

        <el-collapse-item title="个人设置" name="self">
          <el-table v-loading="loading" style="width: 100%">
          <div>
            Operation feedback: enable the users to clearly perceive their
            operations by style updates and interactive effects;
          </div>
          <div>
            Visual feedback: reflect current state by updating or rearranging
            elements of the page.
          </div>
          </el-table>
        </el-collapse-item>

        <el-collapse-item title="群聊设置" name="group">
          <el-table v-loading="loading" style="width: 100%">
          <div>
            Simplify the process: keep operating process simple and intuitive;
          </div>
          <div>
            Definite and clear: enunciate your intentions clearly so that the
            users can quickly understand and make decisions;
          </div>
          <div>
            Easy to identify: the interface should be straightforward, which helps
            the users to identify and frees them from memorizing and recalling.
          </div>
          </el-table>
        </el-collapse-item>

        <el-collapse-item title="实验功能" name="lab">
          <el-table v-loading="loading" style="width: 100%">
          <div>
            Decision making: giving advices about operations is acceptable, but do
            not make decisions for the users;
          </div>
          <div>
            Controlled consequences: users should be granted the freedom to
            operate, including canceling, aborting or terminating current
            operation.
          </div>
          </el-table>
        </el-collapse-item>
      </el-collapse>

    </el-scrollbar>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, toRefs, watch } from 'vue'

/**
 * 聊天设置
 */
const interceptRecall = ref(false) // 反撤回

const activeName = ref('')
const loading = ref(true)
const status = ref("unknown");
const version = ref("unknown");
const moduleVersion = ref("unknown");

if (typeof qwq === "undefined") {
  alert('QwQ bridge is not loaded')
} else {
  qwq.toast('QwQ模块载入成功')
  loading.value = false
  version.value = qwq.getQQVersion()
  status.value = qwq.getStatus()
  moduleVersion.value = qwq.getModuleVersion()
  interceptRecall.value = qwq.mmkvGetValueBoolean('intercept_recall')

  watch(interceptRecall, async (newValue) => {
    qwq.mmkvSetValueBoolean('intercept_recall', newValue)
  })
}

</script>

<style scoped>
.center-vertical {
  position: relative;
}

.settings-text {
  margin-left: 5px; /* 或者根据需要调整 */
  margin-right: 5px;
}

.settings-item-first {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 0px;
  padding-bottom: 10px;
  border-bottom: 1px solid #f0f0f0; /* Adjust the color to match the design */
}

.settings-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 10px;
  padding-bottom: 10px;
  border-bottom: 1px solid #f0f0f0; /* Adjust the color to match the design */
}

.settings-subtext {
  color: #999; /* Adjust the color to match the design */
  font-size: 0.8rem; /* Adjust the font size as needed */
  margin-top: 5px; /* Adjust the margin as needed */
}

</style>
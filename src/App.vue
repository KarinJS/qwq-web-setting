<!--suppress ALL -->
<template>
  <div class="qwq-main">
    <el-tabs
        class="qwq-tabs"
        :stretch="true"
        v-model="activeName"
        @tab-click="handleClick"
    >
      <el-tab-pane
          v-for="tab in tabs"
          :key="tab.name"
          :label="tab.label"
          :name="tab.name"
      ></el-tab-pane>
    </el-tabs>
    <!--    <el-watermark
            :content="['QwQ学习交流', '禁止外传']"
            style="height: 100%;"
        >-->
    <swiper
        class="qwq-swiper"
        :slides-per-view="1"
        @swiper="onSwiper"
        @slideChange="onSlideChange">
      <swiper-slide v-for="tab in tabs" :key="tab.name">
        <div class="qwq-swiper-contents">
          <component :is="tab.content" />
        </div>
      </swiper-slide>
    </swiper>
    <!--      </el-watermark>-->
  </div>
</template>


<script setup lang="ts">
import ChatSetting from "@/components/layouts/ChatSetting.vue";
import SettingMe from "@/components/layouts/SettingMe.vue";
import GroupSetting from "@/components/layouts/GroupSetting.vue";
import MessageSetting from "@/components/layouts/MessageSetting.vue";
import LabSetting from "@/components/layouts/LabSetting.vue";
import StatusLayout from "@/components/layouts/StatusLayout.vue";
import AutoSetting from "@/components/layouts/AutoSetting.vue";
import { ref, reactive, toRefs, watch, onMounted } from 'vue'
import { ElTabs, ElTabPane } from 'element-plus'
import type { TabsPaneContext } from 'element-plus'
import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'

const tabs = ref([
  { label: '状态', name: 'status', content: StatusLayout },
  { label: '聊天设置', name: 'chat', content: ChatSetting },
  { label: '消息设置', name: 'message', content: MessageSetting },
  { label: '群聊设置', name: 'group', content: GroupSetting },
  { label: '个人设置', name: 'me', content: SettingMe },
  { label: '自动分区', name: 'auto', content: AutoSetting },
  { label: '实验室', name: 'lab', content: LabSetting }
])

const activeName = ref('status')
const swiperInstance = ref(null)

const handleClick = (tab: TabsPaneContext, event: Event) => {
  const index = tabs.value.findIndex((t) => t.name === tab.paneName)
  if (index === -1) return
  if (swiperInstance.value.activeIndex === index) return
  swiperInstance.value.slideTo(index)
  //activeName.value = tab.paneName
  //swiperInstance.value.activeIndex = index
}

const onSwiper = (swiper: any) => {
  swiperInstance.value = swiper
};

const onSlideChange = () => {
  let index = swiperInstance.value.activeIndex
  if(activeName.value === tabs.value[index].name) return
  activeName.value = tabs.value[index].name
  //swiperInstance.value.slideTo(index)
};
</script>

<style scoped>
@import "@/assets/qwq.css";

.qwq-main {
  position: fixed;
  top: 0;
  left: 0px;
  width: 100vw;
  height: 100vh;
  overflow: auto;
  padding-left: 10px;
  padding-right: 10px;
  background-color: rgba(0, 0, 0, 0);
}

.qwq-tabs {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.qwq-swiper {
  height: 100%;
  background-color: rgba(0, 0, 0, 0);
}

.qwq-swiper-contents {
  height: 100%;
  margin: 0 2%;
}

.el-tabs {
  display: flex;
  overflow-x: auto;
  white-space: nowrap;
}

.el-tabs__header {
  flex: none;
  display: flex;
}

.el-tabs__content {
  flex: none;
  overflow: auto;
}

.settings-container {
  margin-top: 0;
}
</style>

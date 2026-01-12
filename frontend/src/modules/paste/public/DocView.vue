<script setup>
// DocView组件 - 极简文档预览
// 用于 /view/:slug 路由，只显示内容，无任何导航
// 使用 fixed 定位覆盖 App.vue 的 header
import PasteViewMain from "@/modules/paste/public/components/PasteViewMain.vue";
import { useThemeMode } from "@/composables/core/useThemeMode.js";
import { useAuthStore } from "@/stores/authStore.js";
import { useRouter } from "vue-router";
import { IconRename } from "@/components/icons";

// 定义组件接收的属性
const props = defineProps({
  // 文本分享的唯一标识符
  slug: {
    type: String,
    required: true,
  },
});

const { isDarkMode } = useThemeMode();
const authStore = useAuthStore();
const router = useRouter();

// 跳转到编辑页面
const goToEditPage = () => {
  router.push(`/paste/${props.slug}`);
};
</script>

<template>
  <!-- 极简布局 - fixed 覆盖整个页面，完全没有头部导航 -->
  <div 
    class="doc-view-container fixed inset-0 z-[9999] overflow-auto"
    :class="isDarkMode ? 'bg-custom-bg-900 text-custom-text-dark' : 'bg-custom-bg-50 text-custom-text'"
  >
    <!-- 直接显示内容，无头部 -->
    <main class="pt-6 pb-8">
      <PasteViewMain :dark-mode="isDarkMode" :slug="slug" :minimal-mode="true" />
    </main>

    <!-- 悬浮编辑按钮 - 仅登录用户可见 -->
    <button
      v-if="authStore.isAuthenticated"
      @click="goToEditPage"
      class="fixed bottom-6 right-6 w-14 h-14 rounded-full shadow-lg flex items-center justify-center transition-all hover:scale-110 focus:outline-none focus:ring-4"
      :class="isDarkMode 
        ? 'bg-primary-600 hover:bg-primary-500 text-white focus:ring-primary-500/50' 
        : 'bg-primary-500 hover:bg-primary-600 text-white focus:ring-primary-500/50'"
      title="编辑文档"
    >
      <IconRename size="lg" />
    </button>
  </div>
</template>

<style>
/* 覆盖 Markdown 渲染的 h1 样式，去掉顶部间距 */
.doc-view-container h1 {
  margin-top: 0 !important;
}
</style>

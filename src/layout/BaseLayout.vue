<template>
  <div id="baseLayout">
    <a-layout style="min-height: 100vh">
      <a-layout-header class="header">
        <GlobalHeader />
      </a-layout-header>
      <a-layout>
        <GlobalSider class="sider" />
        <a-layout-content class="content">
          <router-view></router-view>
        </a-layout-content>
      </a-layout>
      <a-layout-footer class="footer"
        ><a href="https://github.com/xwq-image" target="_blank"
          >云图片系统 by ManngerDiao</a
        ></a-layout-footer
      >
    </a-layout>
  </div>
</template>

<script setup lang="ts">
import GlobalHeader from '@/components/GlobalHeader.vue'
import GlobalSider from '@/components/GlobalSider.vue'
</script>

<style scoped>
#baseLayout {
}

#baseLayout .footer {
  background-color: #efefef;
  padding: 16px;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  text-align: center;
}

#baseLayout .content {
  background: linear-gradient(to right, #fefefe, #fff);
  margin-bottom: 80px;
  padding: 28px;
}

#baseLayout .header {
  margin-bottom: 1px;
  background: white;
  color: unset;
  padding-inline: 20px;
}

#baseLayout .sider {
  background: #fff;
  padding-top: 20px;
  border-right: 0.5px solid #eee;
}

#baseLayout :deep(.ant-menu-root) {
  border-bottom: none !important;
  border-inline-end: none !important;
}
</style>

<template>
  <a-config-provider :locale="zh_CN">
    <router-view v-slot="{ Component, route }">
      <component :is="Component" :key="route.path" />
    </router-view>
  </a-config-provider>
</template>

<script lang="ts" setup>
import zh_CN from "ant-design-vue/lib/locale-provider/zh_CN"
import dayjs from "dayjs"
import "dayjs/locale/zh-cn" // import locale

dayjs.locale("zh-cn") // use locale
</script>

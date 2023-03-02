<script setup lang='ts'>
import { onMounted, ref } from 'vue'
import { NSpin } from 'naive-ui'
import { fetchChatConfig } from '@/api'
import pkg from '@/../package.json'

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
  socksProxy?: string
}

const loading = ref(false)

const config = ref<ConfigState>()

async function fetchConfig() {
  try {
    loading.value = true
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  }
  finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchConfig()
})
</script>

<template>
  <NSpin :show="loading">
    <div class="p-4 space-y-4">
      <h2 class="text-xl font-bold">
        Version - {{ pkg.version }}
      </h2>
      <div class="p-2 space-y-2 rounded-md bg-neutral-100 dark:bg-neutral-700">
        <p>
          此网页的提供者为
          <a
            class="text-blue-600 dark:text-blue-500"
            href="https://marlin.red"
            target="_blank"
          >
            Marlin
          </a>
          欢迎您的使用
        </p>
        <p>
          如果你觉得此网页有帮助，可微信打赏
        </p>
      </div>
      <p>API方式：{{ config?.apiModel ?? '-' }}</p>
      <p>反向代理：{{ config?.reverseProxy ?? '-' }}</p>
      <p>超时时间：{{ config?.timeoutMs ?? '-' }}</p>
      <p>Socks代理：{{ config?.socksProxy ?? '-' }}</p>
    </div>
  </NSpin>
</template>

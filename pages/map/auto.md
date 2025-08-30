---
layout: home
title: "自動生成マップ"
description: "自動生成マップ"
---

<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from 'vitepress'
import { getStationByIdLazy } from '../.vitepress/theme/lib/dataLoaders.ts'
import { onMounted, ref } from 'vue'

const route = useRoute()

// 複数パラメータに対応（& 区切り、同名キーは getAll で取得可）
const params = computed(() => {
  void route.path
  if (typeof window === 'undefined') return new URLSearchParams('')
  return new URLSearchParams(window.location.search)
})

const station = computed(() => params.value.get('station') ?? '')

const x = computed(() => {
  const v = params.value.get('x')
  return v != null && v !== '' ? Number(v) : null
})
const y = computed(() => {
  const v = params.value.get('y')
  return v != null && v !== '' ? Number(v) : null
})
const name = computed(() => {
  const v = params.value.get('name')
  return encodeURIComponent(v) != null && v !== '' ? v : null
})

const text = ref('')

onMounted(async () => {
    if (!station.value) return
    console.log(station.value)
    const data = await getStationByIdLazy(station.value)
    console.log(data)
    text.value = data.name
})
</script>

<p style="margin: 0;">{{ text }}</p>

<LinesMap :station="station" :X="x" :Y="y" :shareName="name" />
[場所をシェア](/map/share)
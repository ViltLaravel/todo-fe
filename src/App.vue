<script setup lang="ts">
import Header from '@/component/Header.vue'
import Content from '@/component/Content.vue'
import Layout from '@/layouts/Layout.vue'
</script>

<template>
  <div class="flex justify-center py-[100px] bg-[#B7C9F2] h-screen">
    <Layout>
    <Header />
    <Content />
  </Layout>
  </div>
</template>

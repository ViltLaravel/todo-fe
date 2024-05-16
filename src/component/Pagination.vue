<template>
  <nav v-if="links.length > 3"
    class="flex h-[100px] rounded-b-md items-center justify-between border-t border-gray-200 bg-white px-4 py-3 sm:px-6 shadow-lg"
    aria-label="Pagination">
    <div class="hidden sm:block">
      <p class="text-sm text-gray-700">
        Showing
        {{ ' ' }}
        <span class="font-medium">{{ from }}</span>
        {{ ' ' }}
        to
        {{ ' ' }}
        <span class="font-medium">{{ to }}</span>
        {{ ' ' }}
        of
        {{ ' ' }}
        <span class="font-medium">{{ total }}</span>
        {{ ' ' }}
        results
      </p>
    </div>
    <template v-for="(link, index) in links" :key="index">
      <div class="flex sm:justify-end">
        <div v-if="link.url === null" :key="index"
          class="relative sm:ml-[0px] md:ml-[200px] inline-flex items-center rounded-md bg-white px-3 py-2 text-xs font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus-visible:outline-offset-0"
          v-html="link.label"></div>
        <button v-else :key="index + 1" @click="handlePagination(link.url)"
          class="relative ml-2 inline-flex items-center rounded-md bg-white px-3 py-2 text-xs font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus-visible:outline-offset-0"
          v-html="link.label"></button>
      </div>
    </template>
  </nav>
</template>

<script setup lang="ts">
import type { PropType } from 'vue';
interface Link {
  url: string
  label: string
}

defineProps({
  total: Number,
  to: Number,
  from: Number,
  links: {
    type: Array as PropType<Link[]>,
    required: true
  }
})

const emit = defineEmits(['paginate'])

const handlePagination = (url: string) => {
  if (url !== null && url !== '') {
    const adjustedUrl = url.replace('http://', 'https://');
    emit('paginate', adjustedUrl);
  }
}
</script>

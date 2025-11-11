<script setup lang="ts">
import { useRoute } from 'vue-router';
import { useBookmarkStore } from '@/stores/bookmark.store';
import type { Category } from '@/interfaces/category.inteface';
import { useCategoryStore } from '@/stores/categories.store';
import { ref, watch } from 'vue';
const route = useRoute();
const categoryStore = useCategoryStore();
const bookmarkStore = useBookmarkStore();
const category = ref<Category>();

watch(
  () => ({
    alias: route.params.alias,
    categories: categoryStore.categories,
  }),
  (data) => {
    category.value = categoryStore.getCategoryByAlias(data.alias as string);
    if (category.value) {
      bookmarkStore.fetchBookmarks(category.value.id);
    }
  },
);
</script>

<template>
  Category
  {{ category?.name }}
  {{ bookmarkStore.bookmarks.length }}
</template>

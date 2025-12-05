<script setup lang="ts">
import { useRoute } from 'vue-router';
import { useBookmarkStore } from '@/stores/bookmark.store';
import type { Category } from '@/interfaces/category.inteface';
import { useCategoryStore } from '@/stores/categories.store';
import { onMounted, ref, watch } from 'vue';
import CategoryHeader from '@/components/CategoryHeader.vue';
import BookmarkCard from '@/components/BookmarkCard.vue';
const route = useRoute();
const categoryStore = useCategoryStore();
const bookmarkStore = useBookmarkStore();
const category = ref<Category>();

onMounted(() => {
  category.value = categoryStore.getCategoryByAlias(route.params.alias as string);
  if (category.value) {
    bookmarkStore.fetchBookmarks(category.value.id);
  }
});

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
  <CategoryHeader v-if="category" :category="category" />
  <BookmarkCard
    :id="1"
    image="/avatar.png"
    title="GitHub - gofiber/fiber: ⚡️ Express inspired web framework written in Go"
    url="https://purpleschool.ru"
    :category_id="1"
    :created_at="new Date()"
  />
</template>

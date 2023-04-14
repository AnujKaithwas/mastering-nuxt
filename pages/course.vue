<template>
  <div
    class="p-12 bg-gray-100 w-full h-full min-h-screen flex flex-col items-center"
  >
    <div class="prose mb-12">
      <h1>
        <span class="font-medium">
          Course:
          <span class="font-bold">Mastering Nuxt 3</span>
        </span>
      </h1>
    </div>

    <div class="flex flex-row justify-center flex-grow">
      <div
        class="prose mr-4 p-8 bg-white rounded-md min-w-[20ch] flex flex-col max-w-[30ch]"
      >
        <h3>Chapters</h3>
        <div
          v-for="chapter in chapters"
          class="space-y-1 mb-4 fex flex-col"
          :key="chapter.slug"
        >
          <h4>{{ chapter.title }}</h4>
          <div>
            <NuxtLink
              :to="lesson.path"
              v-for="(lesson, index) in chapter.lessons"
              :key="lesson.slug"
              class="flex flex-row space-x-2 no-underline prose-sm font-normal text-gray-700 hover:text-gray-900"
            >
              <span class="text-gray-500">{{ index + 1 }}.</span>
              <span>{{ lesson.title }}</span>
            </NuxtLink>
          </div>
        </div>
      </div>

      <div class="prose p-12 bg-white rounded-md w-[65ch]">
        <NuxtPage />
      </div>
    </div>
  </div>
</template>

<script setup>
const { chapters } = useCourse();
</script>

<style scoped>
.router-link-active {
  @apply text-blue-500;
}
</style>

<template>
  <div>
    <p class="uppercase mt-0 font-bold text-slate-400 mb-1">
      lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>
    <div>
      <NuxtLink
        v-if="lesson.sourceUrl"
        class="font-normal text-md text-gray-500"
        :to="lesson.sourceUrl"
      >
        Download Source Code
      </NuxtLink>
      <span v-if="lesson.sourceUrl && lesson.downloadUrl">|</span>
      <NuxtLink
        class="font-normal text-md text-gray-500"
        v-if="lesson.downloadUrl"
        :to="lesson.downloadUrl"
        >Download Video</NuxtLink
      >
    </div>
    <VideoPlayer :videoId="lesson.videoId" class="container" />
    <p>{{ lesson.text }}</p>
    <LessonCompleteButton
      :model-value="isLessonComplete"
      @update:model-value="toggleComplete"
    />
  </div>
</template>

<script setup>
const course = useCourse();
const route = useRoute();

const chapter = computed(() => {
  return course.chapters.find(
    (chapter) => chapter.slug === route.params.chapterSlug
  );
});

const lesson = computed(() => {
  return chapter.value.lessons.find(
    (lesson) => lesson.slug === route.params.lessonSlug
  );
});

const title = computed(() => {
  return ` ${chapter.value.title} - ${lesson.value.title}`;
});

useHead({
  title: title,
  meta: [
    {
      hid: "description",
      name: "description",
      content: lesson.value.text,
    },
  ],
});

const progress = useLocalStorage("progress", () => {
  return [];
});

const isLessonComplete = computed(() => {
  if (!progress.value[chapter.value.number - 1]) {
    return false;
  }

  if (!progress.value[chapter.value.number - 1][lesson.value.number - 1]) {
    return false;
  }

  return progress.value[chapter.value.number - 1][lesson.value.number - 1];
});

const toggleComplete = () => {
  if (!progress.value[chapter.value.number - 1]) {
    progress.value[chapter.value.number - 1] = [];
  }

  progress.value[chapter.value.number - 1][lesson.value.number - 1] =
    !isLessonComplete.value;
};
</script>

<style lang="scss" scoped></style>

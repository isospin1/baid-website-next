<template>
  <section class="section">
    <NotFancyTitle cn="新闻" en="News" color="blue"></NotFancyTitle>
    <div
      v-for="(item, index) in news"
      :key="item.id"
      class="w-full border-solid border-[var(--standard-red)] flex h-70 my-10"
      :class="{
        'flex-row-reverse !border-[var(--standard-blue)]': index % 2
      }"
    >
      <img :src="item.cover" alt="" class="w-50% h-full object-cover" />
      <div class="p-10 flex-1 flex flex-col">
        <div class="flex">
          <h2 class="mt-0 flex-1">{{ item.title }}</h2>
          <a
            :href="item.href"
            alt="GOTO"
            class="ml-3 mt--2px color-[var(--standard-red)] op-50 hover:op-100 transition"
            :class="{ '!color-[var(--standard-blue)]': index % 2 }"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="32"
              height="32"
              viewBox="0 0 24 24"
            >
              <path
                fill="currentColor"
                d="M10 6v2H5v11h11v-5h2v6a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V7a1 1 0 0 1 1-1h6zm11-3v9l-3.794-3.793l-5.999 6l-1.414-1.414l5.999-6L12 3h9z"
              /></svg
          ></a>
        </div>
        <p class="op-80">{{ item.description }}</p>
        <div class="flex-1"></div>

        <p class="op-50">
          发布于 {{ item.date.split('T')[0] }}
          {{ item.time.split(':').slice(0, 2).join(':') }}
        </p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import { useRoute } from 'vue-router'
import NotFancyTitle from '../../components/NotFancyTitle.vue'

const route = useRoute()

const news = ref([])

watchEffect(() => {
  fetch(`/${route.params.lang}/db.json`).then((res) => {
    if (res.status === 200) {
      res.json().then((data) => {
        data = Object.values(data)
        // Sort by date
        data.sort((a, b) => {
          return new Date(b.date) - new Date(a.date)
        })
        news.value = data
      })
    }
  })
})
</script>

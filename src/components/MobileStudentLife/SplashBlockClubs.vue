<template>
  <div
    class="flex justify-center items-center flex-col h-50% w-full bg-cover font-sans"
    :style="{
      'background-image': `url(${props.image})`
    }"
  >
    <h2 class="color-white font-title text-10">
      <span>{{ props.title[0] }}</span>
      <span class="color-yellow">{{ props.title[1] }}</span>
      <span>{{ props.title.substr(2) }}</span>
    </h2>
    <span class="decoration-none color-yellow" @click="showPop = true"
      >了解更多</span
    >
    <Transition name="fade" mode="out-in">
      <MobilePopup v-if="showPop" @close="showPop = false">
        <template v-slot:content>
          <swiper
            :modules="[A11y]"
            :slides-per-view="1"
            :space-between="10"
            :loop="true"
            :autoHeight="true"
          >
            <swiper-slide v-for="club in clubs" :key="club">
              <h3 class="text-6">{{ $t(club.name) }}</h3>
              <p class="my-4">
                {{ $t(club.description) }}
              </p>
              <swiper
                :modules="[Autoplay, A11y]"
                :slides-per-view="1"
                :space-between="10"
                :autoplay="{
                  delay: 2500,
                  disableOnInteraction: false
                }"
              >
                <swiper-slide v-for="img in club.images" :key="img">
                  <img
                    :src="img"
                    alt=""
                    class="w-full object-cover aspect-3/2"
                  />
                </swiper-slide>
              </swiper>
            </swiper-slide>
          </swiper>
        </template>
        <!-- <div></div> -->
        <template v-slot:description>
          <span class="text-white m-4">(左右滑动查看更多内容)</span>
        </template>
      </MobilePopup>
    </Transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { clubs } from '../../data/Clubs.js'
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Autoplay, A11y } from 'swiper'

import MobilePopup from '../MobilePopup.vue'

import 'swiper/css'
import 'swiper/css/pagination'

const props = defineProps(['title', 'image']) // TODO: More about dialog
const showPop = ref(false)
</script>

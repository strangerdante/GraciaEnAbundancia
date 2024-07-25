<template>
  <div
    class="mx-auto sm:px-6 lg:px-80 py-10 bg-gray-100 dark:bg-slate-600 transition duration-300 ease-in-out"
  >
    <h2 class="text-3xl px-4 font-bold dark:text-white mb-8">Anuncios</h2>
    <swiper
      :slides-per-view="1"
      :space-between="30"
      :navigation="{
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
      }"
      :pagination="{ clickable: true }"
      :modules="modules"
      :grab-cursor="true"
      :autoplay="{
        delay: 3000,
        disableOnInteraction: false,
      }"
      :effect="'fade'"
      :fade-effect="{ crossFade: true }"
      class="custom-swiper sm:rounded-lg overflow-hidden"
    >
      <swiper-slide v-for="(image, index) in images" :key="index">
        <img
          :src="image"
          :alt="`Slide ${index + 1}`"
          class="w-full h-auto object-cover sm:rounded-lg mb-10"
        />
      </swiper-slide>
      <div class="swiper-button-next custom-swiper-button">
        <i class="fas fa-chevron-right"></i>
      </div>
      <div class="swiper-button-prev custom-swiper-button">
        <i class="fas fa-chevron-left"></i>
      </div>
    </swiper>
  </div>
</template>

<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import { Navigation, Pagination, Autoplay, EffectFade } from "swiper/modules";
import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";
import "swiper/css/autoplay";
import "swiper/css/effect-fade";

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      modules: [Navigation, Pagination, Autoplay, EffectFade],
      images: [
        "https://i.ibb.co/RzRrTRX/1.jpg",
        "https://i.ibb.co/bKfVw1Y/2.jpg",
        "https://i.ibb.co/k8SpDdf/3.jpg",
        "https://i.ibb.co/q0378j2/4.jpg",
        "https://i.ibb.co/HFh8qjT/5.jpg",
        "https://i.ibb.co/gP4F0Mn/6.jpg",
      ],
    };
  },
};
</script>

<style lang="postcss">
.custom-swiper {
  .custom-swiper-button {
    @apply text-white bg-blue-500 rounded-full w-10 h-10 flex items-center justify-center opacity-75 transition-opacity duration-300;
    &:hover {
      @apply opacity-100;
    }
    &::after {
      content: none;
    }
  }
  .swiper-button-next {
    @apply right-4;
  }
  .swiper-button-prev {
    @apply left-4;
  }
  .swiper-pagination-bullet {
    @apply bg-blue-400;
    width: 10px;
    height: 10px;
  }
  .swiper-pagination-bullet-active {
    @apply bg-blue-500;
    width: 12px;
    height: 12px;
  }
}
</style>

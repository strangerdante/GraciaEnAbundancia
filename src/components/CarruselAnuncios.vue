<template>
  <div
    class="mx-auto sm:px-6 lg:px-80 py-10 bg-gray-100 dark:bg-slate-600 transition duration-300 ease-in-out"
  >
    <h2 class="text-3xl px-4 font-bold dark:text-white mb-8">Anuncios</h2>
    <swiper
      :slides-per-view="1"
      :space-between="30"
      :navigation="true"
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
  .swiper-button-next,
  .swiper-button-prev {
    @apply text-teal-500;
  }
  .swiper-button-next:hover,
  .swiper-button-prev:hover {
    @apply text-teal-400;
  }
  .swiper-pagination-bullet {
    @apply bg-teal-400;
    width: 10px;
    height: 10px;
  }
  .swiper-pagination-bullet-active {
    @apply bg-teal-500;
    width: 10px;
    height: 10px;
  }
}
</style>

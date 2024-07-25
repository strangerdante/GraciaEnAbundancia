<template>
  <div
    class="mx-auto sm:px-6 lg:px-80 py-6 sm:py-10 bg-gray-100 dark:bg-slate-600 transition duration-300 ease-in-out"
  >
    <h2
      class="text-2xl sm:text-3xl px-4 font-bold dark:text-white mb-6 sm:mb-8"
    >
      Anuncios
    </h2>
    <swiper
      :slides-per-view="1"
      :space-between="30"
      :navigation="{
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
        hideOnMobile: true,
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
      <swiper-slide v-for="(slide, index) in slides" :key="index">
        <div class="relative">
          <img
            :src="slide.image"
            :alt="`Slide ${index + 1}`"
            class="w-full h-[250px] sm:h-[600px] sm:rounded-lg mb-10 object-cover"
          />
          <div
            class="absolute inset-0 flex flex-col items-center justify-center text-white bg-black bg-opacity-50"
          >
            <h3 class="text-xl sm:text-2xl font-bold mb-2 sm:mb-4">
              {{ slide.title }}
            </h3>
            <p class="text-sm sm:text-lg mb-4 sm:mb-6 px-16">
              {{ slide.description }}
            </p>
            <button
              class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
              @click="handleButtonClick(slide.buttonLink)"
            >
              {{ slide.buttonText }}
            </button>
          </div>
        </div>
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
      slides: [
        {
          image:
            "https://i.ibb.co/9Td3wVk/ben-white-W8-Qqn1-Pm-QH0-unsplash.jpg",
          title: "Culto de oración",
          description:
            "Únete a nosotros cada miércoles a las 7 PM para un tiempo de oración comunitaria y fortalecimiento espiritual. Juntos, elevaremos nuestras voces al cielo.",
          buttonText: "Leer más",
          buttonLink: "/pagina1",
        },
        {
          image:
            "https://i.ibb.co/KW60XdT/aaron-burden-9zs-HNt5-Opq-E-unsplash.jpg",
          title: "Reunión de varones",
          description:
            "Caballeros, los invitamos a nuestra reunión mensual este sábado. Compartiremos testimonios, estudiaremos la Palabra y nos apoyaremos mutuamente en nuestro caminar cristiano.",
          buttonText: "Ver detalles",
          buttonLink: "/pagina2",
        },
        {
          image:
            "https://i.ibb.co/KW60XdT/aaron-burden-9zs-HNt5-Opq-E-unsplash.jpg",
          title: "Reunión de damas",
          description:
            "Queridas hermanas, las esperamos en nuestra reunión especial este viernes. Tendremos un tiempo de alabanza, estudio bíblico y compañerismo diseñado específicamente para mujeres de fe.",
          buttonText: "Ver detalles",
          buttonLink: "/pagina2",
        },
      ],
    };
  },
  methods: {
    handleButtonClick(link) {
      console.log(`Navegando a: ${link}`);
      // this.$router.push(link); // Descomenta esta línea si estás usando vue-router
    },
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
    /* Ocultar en móviles */
    @apply hidden sm:flex;
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

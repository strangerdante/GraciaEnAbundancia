<template>
  <section>
    <div
      class="mx-auto sm:px-6 2xl:px-80 py-6 sm:py-10 bg-gray-100 dark:bg-slate-600 transition duration-300 ease-in-out"
    >
      <div class="flex justify-between items-center mb-2">
        <h2 class="text-3xl font-bold dark:text-white ml-1">Anuncios</h2>
      </div>

      <div v-if="error" class="text-red-500 text-center mb-4">{{ error }}</div>
      <div
        v-else-if="isLoading"
        class="flex flex-col justify-center items-center h-64"
      >
        <div class="loader mb-4"></div>
        <p class="text-gray-700 dark:text-gray-300">Cargando anuncios...</p>
      </div>
      <swiper
        v-else-if="slides.length"
        :slides-per-view="1"
        :space-between="30"
        :navigation="{
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev',
          hideOnMobile: true,
        }"
        :pagination="{ clickable: true }"
        :modules="modules"
        :loop="true"
        :grab-cursor="true"
        :autoplay="{
          delay: 5000,
          disableOnInteraction: false,
        }"
        :effect="'creative'"
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
              v-if="slide.titulo || slide.descripcion || slide.textoBoton"
              class="absolute inset-0 flex flex-col items-center justify-evenly text-white bg-black bg-opacity-50"
            >
              <h3
                v-if="slide.titulo"
                class="text-2xl sm:text-3xl font-bold mb-2 sm:mb-4 text-center"
              >
                {{ slide.titulo }}
              </h3>
              <p
                v-if="slide.descripcion"
                class="text-base sm:text-2xl mb-4 sm:mb-6 px-4 sm:px-24 text-center text-white font-medium drop-shadow-lg bg-black/30 rounded-lg p-2"
              >
                {{ slide.descripcion }}
              </p>
              <button
                v-if="slide.textoBoton"
                class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded text-sm sm:text-base"
                @click="handleButtonClick(slide.linkBoton)"
              >
                {{ slide.textoBoton }}
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

      <div v-else class="text-center py-4">No hay anuncios disponibles</div>
    </div>
  </section>
</template>

<script>
import { ref, onMounted } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { Navigation, Pagination, Autoplay, EffectFade } from "swiper/modules";
import "swiper/css";
import "swiper/css/navigation";
import "swiper/css/pagination";
import "swiper/css/autoplay";
import "swiper/css/effect-fade";
import { eventos } from "../lib/api";

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  setup() {
    const slides = ref([]);
    const error = ref("");
    const isLoading = ref(false);
    const modules = [Navigation, Pagination, Autoplay, EffectFade];

    const loadEvents = async () => {
      try {
        isLoading.value = true;
        const response = await eventos.getAll();

        // Ordena los eventos por ID en orden descendente
        const sortedEvents = response.data.sort((a, b) => b.id - a.id);

        // Mapea los eventos ordenados al formato del carrusel
        slides.value = sortedEvents.map((evento) => ({
          image: evento.image, // Placeholder si no hay imagen
          titulo: evento.titulo,
          descripcion: evento.descripcion,
          textoBoton: evento.textoBoton,
          linkBoton: evento.linkBoton || "#",
        }));
      } catch (err) {
        error.value = "Error al cargar los anuncios";
        console.error(err);
      } finally {
        isLoading.value = false;
      }
    };

    const handleButtonClick = (link) => {
      console.log(`Navegando a: ${link}`);
    };

    onMounted(loadEvents);

    return {
      slides,
      error,
      isLoading,
      modules,
      handleButtonClick,
    };
  },
};
</script>

<style lang="postcss">
.loader {
  width: 50px;
  padding: 8px;
  aspect-ratio: 1;
  border-radius: 50%;
  background: #3b82f6;
  --_m: conic-gradient(#0000 10%, #000), linear-gradient(#000 0 0) content-box;
  -webkit-mask: var(--_m);
  mask: var(--_m);
  -webkit-mask-composite: source-out;
  mask-composite: subtract;
  animation: l3 1s infinite linear;
}
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

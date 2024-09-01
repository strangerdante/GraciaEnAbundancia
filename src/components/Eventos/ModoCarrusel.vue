<template>
  <div class="relative">
    <swiper
      :modules="modulos"
      :slides-per-view="2"
      :space-between="4"
      :pagination="{ clickable: true }"
      :navigation="false"
      :grab-cursor="false"
      class="mySwiper custom-swiper rounded-lg overflow-hidden"
      :breakpoints="{
        '420': {
          slidesPerView: 2,
          spaceBetween: 7,
        },
        '768': {
          slidesPerView: 3,
          spaceBetween: 20,
        },
        '1020': {
          slidesPerView: 4,
          spaceBetween: 20,
        },
      }"
    >
      <swiper-slide v-for="evento in eventos" :key="evento.fecha">
        <div
          class="p-[2px] rounded-lg mb-10 mx-auto group relative overflow-hidden"
        >
          <div
            class="absolute inset-0 dark:bg-gradient-to-tr from-blue-500 to-teal-500 rounded-lg animate-gradient"
          ></div>
          <div
            class="bg-white dark:bg-slate-600/85 rounded-lg shadow flex flex-col h-[340px] md:h-[330px] relative z-10"
          >
            <div class="flex-grow py-1 px-2 sm:px-6">
              <div class="flex items-center justify-center p-4">
                <div
                  :class="[
                    'relative text-3xl font-bold text-black border py-2 px-6 rounded-md shadow-md bg-white folded-corner',
                    evento.infoIconoTexto === 'Canasta de amor'
                      ? 'border-t-red-500'
                      : '',
                    evento.infoIconoTexto === 'Cena del Señor'
                      ? 'border-t-red-700'
                      : '',
                    evento.infoIconoTexto === 'Reunión de damas'
                      ? 'border-t-pink-500'
                      : '',
                    evento.infoIconoTexto === 'Domingo misionero'
                      ? 'border-t-green-500'
                      : '',
                    evento.infoIconoTexto === 'Culto de oración'
                      ? 'border-t-violet-500'
                      : '',
                    evento.infoIconoTexto !== 'Canasta de amor' &&
                    evento.infoIconoTexto !== 'Cena del Señor' &&
                    evento.infoIconoTexto !== 'Reunión de damas' &&
                    evento.infoIconoTexto !== 'Domingo misionero' &&
                    evento.infoIconoTexto !== 'Culto de oración'
                      ? 'border-t-blue-500'
                      : '',
                    'border-t-4',
                    'transition-transform duration-300 group-hover:scale-105',
                  ]"
                >
                  <div
                    class="absolute top-1 left-3 w-1.5 h-1.5 bg-black rounded-full"
                  ></div>
                  <div
                    class="absolute top-1 right-3 w-1.5 h-1.5 bg-black rounded-full"
                  ></div>
                  <div class="text-center">{{ evento.dia }}</div>
                  <div class="w-full h-[1px] bg-gray-300 my-1"></div>
                  <div class="text-sm text-gray-600 text-center">
                    {{ evento.mes }}
                  </div>
                  <InfoIcono
                    :show="evento.infoAdiccional"
                    size="small"
                    :texto="evento.infoIconoTexto"
                  />
                </div>
              </div>
              <h3
                class="font-semibold mb-2 text-[14px] xl:text-xl flex items-center truncate dark:text-white"
              >
                {{ evento.titulo }}
                <svg
                  v-if="evento.banner !== null"
                  class="ml-2 w-4 h-4 text-gray-600"
                  viewBox="0 0 24 24"
                  fill="white"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
                  <circle cx="8.5" cy="8.5" r="1.5" />
                  <polyline points="21 15 16 10 5 21" />
                </svg>
              </h3>
              <p
                class="text-sm text-gray-600 mb-2 flex items-center dark:text-white"
              >
                <i class="fas fa-clock mr-2"></i>{{ evento.diaSemana }},
                {{ evento.hora }}
              </p>
              <p
                class="text-[13px] sm:text-sm text-gray-600 dark:text-white mb-2 flex"
              >
                <i
                  :class="{
                    'fas fa-map-marker-alt': evento.lugar,
                    'fas fa-link': evento.link,
                  }"
                  class="mr-2 mt-1 flex-shrink-0"
                ></i>
                <span class="mr-[2px] sm:mr-[6px] break-all">{{
                  evento.lugar || evento.link
                }}</span>
              </p>
              <p
                class="text-[13px] sm:text-sm font-semibold text-gray-600 mb-2 dark:text-white"
              >
                <i class="fas fa-calendar-plus mr-2"></i>
                {{
                  evento.diasRestantes === 0
                    ? "Hoy"
                    : evento.diasRestantes === 1
                    ? "1 día restante"
                    : `${evento.diasRestantes} días restantes`
                }}
              </p>
            </div>
            <div class="mt-auto pb-3 px-2 sm:px-6">
              <button
                @click="$emit('abrir-modal', evento)"
                class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition duration-300 dark:bg-teal-500 dark:hover:bg-teal-700 transform hover:-translate-y-1"
              >
                Detalles
              </button>
            </div>
          </div>
        </div>
      </swiper-slide>
    </swiper>
  </div>
</template>

<script>
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination, Navigation } from "swiper/modules";
import "swiper/css";
import "swiper/css/pagination";
import "swiper/css/navigation";
import InfoIcono from "./InfoIcono.vue";

export default {
  components: {
    Swiper,
    SwiperSlide,
    InfoIcono,
  },
  props: {
    eventos: {
      type: Array,
      required: true,
    },
  },
  setup() {
    return {
      modulos: [Pagination, Navigation],
    };
  },
};
</script>

<style scoped>
.folded-corner {
  position: relative;
}

.folded-corner::after {
  content: "";
  position: absolute;
  bottom: 0;
  right: 0;
  width: 0;
  height: 0;
  border-radius: 0 0 5px 0;
  border-style: solid;
  border-width: 0 0 17px 17px;
  border-color: transparent transparent #e0e0e0 transparent;
  box-shadow: -2px -2px 3px rgba(0, 0, 0, 0.1);
}
</style>

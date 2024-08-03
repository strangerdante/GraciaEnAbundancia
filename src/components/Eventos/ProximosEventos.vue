<template>
  <div
    class="bg-gray-100 dark:bg-slate-600 px-1 sm:px-6 lg:px-80 pb-10 transition duration-300 ease-in-out"
  >
    <div class="flex justify-between items-center mb-8 px-3">
      <h2 class="text-3xl font-bold dark:text-white">Próximos eventos</h2>
      <button
        @click="toggleView"
        class="text-2xl text-gray-700 dark:text-teal-500"
      >
        <i :class="isCarouselView ? 'fas fa-list' : 'fas fa-th'"></i>
      </button>
    </div>

    <!-- Estado de carga -->
    <div v-if="cargando" class="flex flex-col justify-center items-center h-64">
      <div class="loader mb-4"></div>
      <p class="text-gray-700 dark:text-gray-300">Cargando eventos...</p>
    </div>

    <!-- Mensaje de error -->
    <div v-else-if="error" class="text-red-500 text-center py-8">
      {{ error }}
    </div>

    <!-- Contenido principal -->
    <div v-else>
      <!-- Vista de carrusel -->
      <div v-if="isCarouselView" class="relative">
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
              class="bg-white py-4 px-2 sm:px-6 rounded-lg shadow mx-auto mb-10"
            >
              <div class="flex items-center justify-center p-4">
                <div
                  :class="[
                    'relative text-3xl font-bold text-black border py-2 px-6 rounded-md shadow-md',
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
                  ]"
                >
                  <div
                    class="absolute top-1 left-3 w-1.5 h-1.5 bg-black rounded-full"
                  ></div>
                  <div
                    class="absolute top-1 right-3 w-1.5 h-1.5 bg-black rounded-full"
                  ></div>
                  <div class="text-center">{{ evento.dia }}</div>
                  <div class="text-sm text-gray-600 text-center">
                    {{ evento.mes }}
                  </div>
                  <!-- Badge -->
                  <InfoIcono
                    :show="evento.infoAdiccional"
                    size="small"
                    :texto="evento.infoIconoTexto"
                  />
                </div>
              </div>
              <!-- Indicador de banner en el titulo -->
              <h3
                class="font-semibold mb-2 text-[14px] xl:text-xl flex items-center"
              >
                {{ evento.titulo }}
                <svg
                  v-if="evento.banner !== null"
                  class="ml-2 w-4 h-4 text-gray-600"
                  viewBox="0 0 24 24"
                  fill="none"
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

              <p class="text-sm text-gray-600 mb-2 flex items-center">
                <i class="fas fa-clock mr-2"></i
                >{{ obtenerDiaSemana(evento.fecha) }}, {{ evento.hora }}
              </p>
              <p class="text-sm text-gray-600 mb-2 flex items-center">
                <i class="fas fa-map-marker-alt mr-2"></i> {{ evento.lugar }}
              </p>
              <p
                class="text-sm font-semibold text-gray-600 mb-4 dark:text-teal-600"
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
              <button
                @click="abrirModal(evento)"
                class="bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition duration-300 dark:bg-teal-500 dark:hover:bg-teal-700"
              >
                Detalles
              </button>
            </div>
          </swiper-slide>
        </swiper>
      </div>

      <!-- Vista de lista -->
      <div v-else class="grid grid-cols-1 md:grid-cols-7 gap-4">
        <!-- Evento próximo destacado -->
        <div
          class="md:col-span-4 bg-white p-6 rounded-lg shadow-lg hover:shadow-xl transition-shadow duration-300"
        >
          <div
            class="flex flex-col md:flex-row items-start md:items-center mb-4"
          >
            <div
              :class="[
                'text-5xl font-bold text-black py-3 px-8 rounded-md shadow-md mr-6 mb-4 md:mb-0 relative',
                proximoEvento.infoIconoTexto === 'Canasta de amor'
                  ? 'border-t-red-500'
                  : '',
                proximoEvento.infoIconoTexto === 'Cena del Señor'
                  ? 'border-t-red-700'
                  : '',
                proximoEvento.infoIconoTexto === 'Reunión de damas'
                  ? 'border-t-pink-500'
                  : '',
                proximoEvento.infoIconoTexto === 'Domingo misionero'
                  ? 'border-t-green-500'
                  : '',
                proximoEvento.infoIconoTexto === 'Culto de oración'
                  ? 'border-t-violet-500'
                  : '',
                proximoEvento.infoIconoTexto !== 'Canasta de amor' &&
                proximoEvento.infoIconoTexto !== 'Cena del Señor' &&
                proximoEvento.infoIconoTexto !== 'Reunión de damas' &&
                proximoEvento.infoIconoTexto !== 'Domingo misionero' &&
                proximoEvento.infoIconoTexto !== 'Culto de oración'
                  ? 'border-t-blue-500'
                  : '',
                'border-t-4',
              ]"
            >
              <div class="text-center">{{ proximoEvento.dia }}</div>
              <div class="text-lg text-gray-600 text-center">
                {{ proximoEvento.mes }}
              </div>
              <!-- Badge -->
              <InfoIcono
                :show="proximoEvento.infoAdiccional"
                size="medium"
                :texto="proximoEvento.infoIconoTexto"
              />
            </div>
            <div class="flex-grow">
              <h2
                class="font-bold text-xl md:text-2xl mb-2 text-gray-700 flex items-center"
              >
                {{ proximoEvento.titulo }}
                <svg
                  v-if="proximoEvento.banner !== null"
                  class="ml-2 w-5 h-5 text-gray-600"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
                  <circle cx="8.5" cy="8.5" r="1.5" />
                  <polyline points="21 15 16 10 5 21" />
                </svg>
              </h2>
              <p
                class="text-base md:text-lg text-gray-600 flex items-center mb-2"
              >
                <i class="fas fa-calendar-alt mr-2"></i>
                {{ obtenerDiaSemana(proximoEvento.fecha) }},
                {{ proximoEvento.hora }}
              </p>
              <p
                class="text-base md:text-lg text-gray-600 flex items-center mb-2"
              >
                <i class="fas fa-map-marker-alt mr-2"></i>
                {{ proximoEvento.lugar }}
              </p>
              <p class="text-base md:text-lg my-2 text-gray-700 line-clamp-2">
                {{ proximoEvento.descripcion }}
              </p>

              <!-- Banner disponible o no disponible -->
              <div v-if="proximoEvento.banner">
                <a
                  href="#"
                  @click.prevent="abrirModal(proximoEvento)"
                  class="text-blue-600 hover:text-blue-800 text-sm cursor-pointer flex items-center"
                >
                  Banner disponible
                  <svg
                    class="ml-2 w-4 h-4"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
                    <circle cx="8.5" cy="8.5" r="1.5" />
                    <polyline points="21 15 16 10 5 21" />
                  </svg>
                </a>
              </div>
              <div v-else>
                <a
                  href="#"
                  @click.prevent="abrirModal(proximoEvento)"
                  class="text-red-500 text-sm cursor-pointer flex items-center"
                >
                  Banner no disponible
                  <svg
                    class="ml-2 w-4 h-4"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
                    <circle cx="8.5" cy="8.5" r="1.5" />
                    <polyline points="21 15 16 10 5 21" />
                  </svg>
                </a>
              </div>
              <p
                class="text-base md:text-lg font-semibold text-red-600 mt-4 dark:text-teal-600 flex items-center"
              >
                <i class="fas fa-hourglass-half mr-2"></i>
                {{
                  proximoEvento.diasRestantes === 0
                    ? "Hoy"
                    : proximoEvento.diasRestantes === 1
                    ? "1 día restante"
                    : `${proximoEvento.diasRestantes} días restantes`
                }}
              </p>
            </div>
          </div>
          <!-- 
espacio organizador -->
        </div>

        <!-- Lista de eventos adicionales -->
        <div class="md:col-span-3 space-y-4">
          <div
            v-for="evento in eventosAdicionales"
            :key="evento.fecha"
            class="bg-white p-4 rounded-lg shadow flex items-center justify-between"
          >
            <div class="flex items-center">
              <div
                :class="[
                  'text-2xl font-bold text-black border py-2 px-4 rounded-md shadow-md mr-4 relative',
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
                ]"
              >
                <div class="text-center">{{ evento.dia }}</div>
                <div class="text-xs text-gray-600 text-center">
                  {{ evento.mes }}
                </div>
                <!-- Badge -->
                <InfoIcono
                  :show="evento.infoAdiccional"
                  size="small"
                  :texto="evento.infoIconoTexto"
                />
              </div>
              <div>
                <!-- Indicador de banner en el titulo -->
                <h3 class="text-sm font-semibold flex items-center">
                  {{ evento.titulo }}
                  <svg
                    v-if="evento.banner !== null"
                    class="ml-2 w-4 h-4 text-gray-600"
                    viewBox="0 0 24 24"
                    fill="none"
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
                <p class="text-sm text-gray-600">
                  {{ obtenerDiaSemana(evento.fecha) }}, {{ evento.hora }} -
                  {{ evento.lugar }}
                </p>
              </div>
            </div>
            <button
              @click="abrirModal(evento)"
              class="bg-blue-500 dark:bg-teal-500 text-white px-3 py-1 rounded-lg hover:bg-blue-700 transition duration-300 text-sm"
            >
              Detalles
            </button>
          </div>
        </div>
      </div>
    </div>
    <EventoModal
      v-if="eventoSeleccionado"
      :evento="eventoSeleccionado"
      @cerrar="cerrarModal"
    />
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination, Navigation } from "swiper/modules";
import "swiper/css";
import "swiper/css/pagination";
import "swiper/css/navigation";
import InfoIcono from "./InfoIcono.vue";
import EventoModal from "./EventoModal.vue";

export default {
  components: {
    Swiper,
    SwiperSlide,
    InfoIcono,
    EventoModal,
  },
  setup() {
    const eventos = ref([]);
    const eventoSeleccionado = ref(null);
    const cargando = ref(true);
    const error = ref(null);
    const isCarouselView = ref(true);

    const toggleView = () => {
      isCarouselView.value = !isCarouselView.value;
    };

    const abrirModal = (evento) => {
      eventoSeleccionado.value = evento;
    };

    const cerrarModal = () => {
      eventoSeleccionado.value = null;
    };

    const obtenerDiaSemana = (fecha) => {
      const dias = [
        "Domingo",
        "Lunes",
        "Martes",
        "Miércoles",
        "Jueves",
        "Viernes",
        "Sábado",
      ];
      return dias[fecha.getUTCDay()];
    };

    const proximoEvento = computed(() => {
      return eventos.value[0] || null;
    });

    const eventosAdicionales = computed(() => {
      return eventos.value.slice(1, 4);
    });

    const generarEventos = (startDate, endDate) => {
      // Genera eventos recurrentes para el período especificado (8 días)
      const events = [];
      const currentDate = new Date(startDate);

      while (currentDate <= endDate) {
        const year = currentDate.getUTCFullYear();
        const month = currentDate.getUTCMonth();
        const date = currentDate.getUTCDate();
        const day = currentDate.getUTCDay();

        // Eventos dominicales
        if (day === 0) {
          const weekOfMonth = Math.floor((date - 1) / 7) + 1;
          let infoIconoTexto = "Servicio dominical";

          if (weekOfMonth === 1) infoIconoTexto = "Cena del Señor";
          else if (weekOfMonth === 3) infoIconoTexto = "Canasta de amor";
          else if (date > 24) infoIconoTexto = "Domingo misionero";

          events.push({
            id: `domingo-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Servicio dominical",
            hora: "8:00 am — 12:30 pm",
            lugar: "Carrera 35 #1C-30",
            descripcion: "Servicio dominical semanal.",
            infoAdiccional: true,
            infoIconoTexto: infoIconoTexto,
            banner: null,
          });
        }

        // Eventos del último sábado del mes
        if (day === 6 && date > 24) {
          events.push({
            id: `varones-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Reunión de varones",
            hora: "3:30 pm - 5:30 pm",
            lugar: "Por confirmar",
            descripcion: "Reunión mensual de varones.",
            infoAdiccional: true,
            infoIconoTexto: "Reunión de varones",
            banner: null,
          });

          events.push({
            id: `damas-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Reunión de damas",
            hora: "3:30 pm - 5:30 pm",
            lugar: "Por confirmar",
            descripcion: "Reunión mensual de damas.",
            infoAdiccional: true,
            infoIconoTexto: "Reunión de damas",
            banner: null,
          });
        }

        // Eventos de los miércoles
        if (day === 3) {
          events.push({
            id: `oracion-${year}-${month + 1}-${date}`,
            fecha: new Date(Date.UTC(year, month, date)),
            titulo: "Culto de oración",
            hora: "7:00 pm - 8:00 pm",
            lugar: "Reunión virtual",
            descripcion: "Culto semanal de oración.",
            infoAdiccional: true,
            infoIconoTexto: "Culto de oración",
            banner: null,
          });
        }

        currentDate.setUTCDate(currentDate.getUTCDate() + 1);
      }

      return events;
    };

    const fusionarEventos = (eventosGenerados, eventosAPI) => {
      const eventosMap = new Map();

      // Agrega todos los eventos generados al mapa
      eventosGenerados.forEach((evento) => {
        const key = `${evento.fecha.toISOString().split("T")[0]}-${evento.id}`;
        eventosMap.set(key, evento);
      });

      // Sobreescribe o agrega los eventos de la API
      eventosAPI.forEach((eventoAPI) => {
        const fecha = new Date(eventoAPI.fecha);
        const key = `${eventoAPI.fecha}-${eventoAPI.id || ""}`;

        if (eventosMap.has(key)) {
          // Sobreescribe el evento existente
          eventosMap.set(key, {
            ...eventosMap.get(key),
            ...eventoAPI,
            fecha: fecha,
          });
        } else {
          // Agrega el nuevo evento de la API
          eventosMap.set(key, {
            ...eventoAPI,
            fecha: fecha,
          });
        }
      });

      // Convierte el mapa de vuelta a un array y ordena los eventos
      return Array.from(eventosMap.values()).sort(
        (a, b) => a.fecha - b.fecha || a.titulo.localeCompare(b.titulo)
      );
    };

    onMounted(async () => {
      try {
        cargando.value = true;
        const today = new Date();
        today.setUTCHours(0, 0, 0, 0);
        const endDate = new Date(today);
        endDate.setUTCDate(endDate.getUTCDate() + 15); // Eventos de 15 días

        const eventosGenerados = generarEventos(today, endDate);

        // Obtiene eventos de la API
        const respuesta = await fetch("/eventos.json");
        if (!respuesta.ok) {
          throw new Error("Error al obtener los eventos");
        }
        const eventosAPI = await respuesta.json();

        // Fusiona los eventos generados con los de la API
        const eventosFusionados = fusionarEventos(eventosGenerados, eventosAPI);

        // Procesa los eventos fusionados
        eventos.value = eventosFusionados.map((evento) => ({
          ...evento,
          dia: evento.fecha.getUTCDate().toString().padStart(2, "0"),
          mes: evento.fecha.toLocaleString("es", {
            month: "long",
            timeZone: "UTC",
          }),
          diasRestantes: Math.ceil(
            (evento.fecha - today) / (1000 * 60 * 60 * 24)
          ),
          diaSemana: obtenerDiaSemana(evento.fecha),
        }));
      } catch (err) {
        console.error("Error al cargar los eventos:", err);
        error.value =
          "No se pudieron cargar los eventos. Por favor, intente más tarde.";
      } finally {
        cargando.value = false;
      }
    });

    return {
      modulos: [Pagination, Navigation],
      eventos,
      eventoSeleccionado,
      abrirModal,
      cerrarModal,
      cargando,
      error,
      obtenerDiaSemana,
      isCarouselView,
      toggleView,
      proximoEvento,
      eventosAdicionales,
    };
  },
};
</script>

<style scoped>
.loader {
  width: 50px;
  padding: 8px;
  aspect-ratio: 1;
  border-radius: 50%;
  background: #25b09b;
  --_m: conic-gradient(#0000 10%, #000), linear-gradient(#000 0 0) content-box;
  -webkit-mask: var(--_m);
  mask: var(--_m);
  -webkit-mask-composite: source-out;
  mask-composite: subtract;
  animation: l3 1s infinite linear;
}
@keyframes l3 {
  to {
    transform: rotate(1turn);
  }
}

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

<template>
  <div
    class="bg-gray-100 dark:bg-slate-600 px-4 sm:px-6 lg:px-80 pb-10 transition duration-300 ease-in-out selection:bg-teal-500 selection:text-white"
  >
    <div class="flex justify-between items-center mb-8">
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
          :space-between="7"
          :pagination="{ clickable: true }"
          :navigation="false"
          :grab-cursor="true"
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
                  class="relative text-3xl font-bold text-black border py-2 px-6 rounded-md shadow-md border-t-teal-500 border-t-4"
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
                </div>
              </div>
              <h3 class="font-semibold mb-2 text-[16px] xl:text-xl">
                {{ evento.titulo }}
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
                class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition duration-300 dark:bg-teal-500 dark:hover:bg-teal-700"
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
              class="text-5xl font-bold text-black border py-3 px-8 rounded-md shadow-md mr-6 mb-4 md:mb-0 border-t-teal-500 border-t-4"
            >
              <div class="text-center">{{ proximoEvento.dia }}</div>
              <div class="text-lg text-gray-600 text-center">
                {{ proximoEvento.mes }}
              </div>
            </div>
            <div class="flex-grow">
              <h2 class="font-bold text-xl md:text-2xl mb-2 text-gray-700">
                {{ proximoEvento.titulo }}
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

              <!-- Leer mas -->
              <a href="#" class="text-blue-600 hover:text-blue-800 text-sm"
                >Detalles</a
              >
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
          <!-- Organizador -->
          <!-- <div class="mt-4 flex items-center">
            <img
              src="https://i.ibb.co/z4bPJTy/servicio.jpg"
              alt="Organizador"
              class="w-10 h-10 rounded-full mr-2"
            />
            <div>
              <p class="font-semibold text-sm">Organizado por</p>
              <p class="text-gray-600 text-sm">Nombre del Organizador</p>
            </div>
          </div> -->

          <!-- compartir asistentes -->
          <!-- <div class="mt-4 flex justify-between items-center">
            <button
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded transition duration-300 ease-in-out flex items-center"
            >
              <i class="fas fa-calendar-plus mr-2"></i> Añadir al calendario
            </button>
            <span class="text-gray-600">
              <i class="fas fa-users mr-2"></i> 42 asistentes
            </span>
          </div> -->
        </div>

        <!-- Lista de eventos adicionales -->
        <div class="md:col-span-3 space-y-4">
          <div
            v-for="evento in eventosAdicionales"
            :key="evento.fecha"
            class="bg-white p-4 rounded-lg shadow flex items-center justify-between"
          >
            <div class="flex items-center">
              <!-- cambiar colores -->
              <div
                class="text-2xl font-bold text-black border py-2 px-4 rounded-md shadow-md mr-4 border-t-teal-500 border-t-4"
              >
                <div class="text-center">{{ evento.dia }}</div>
                <div class="text-xs text-gray-600 text-center">
                  {{ evento.mes }}
                </div>
              </div>
              <div>
                <h3 class="font-semibold">{{ evento.titulo }}</h3>
                <p class="text-sm text-gray-600">
                  {{ obtenerDiaSemana(evento.fecha) }}, {{ evento.hora }} -
                  {{ evento.lugar }}
                </p>
              </div>
            </div>
            <button
              @click="abrirModal(evento)"
              class="bg-red-500 dark:bg-teal-500 text-white px-3 py-1 rounded-lg hover:bg-red-700 transition duration-300 text-sm"
            >
              Detalles
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div
      v-if="eventoSeleccionado"
      class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
    >
      <div class="bg-white p-8 rounded-lg shadow-xl max-w-md w-full">
        <h2 class="text-2xl font-bold mb-4">
          {{ eventoSeleccionado.titulo }}
        </h2>
        <p class="mb-2">
          <strong>Fecha:</strong> {{ eventoSeleccionado.dia }} de
          {{ eventoSeleccionado.mes }}
        </p>
        <p class="mb-2"><strong>Hora:</strong> {{ eventoSeleccionado.hora }}</p>
        <p class="mb-2">
          <strong>Lugar:</strong> {{ eventoSeleccionado.lugar }}
        </p>

        <p class="mb-2">
          <strong>Descripción:</strong> {{ eventoSeleccionado.descripcion }}
        </p>
        <p class="mb-2 text-red-600 dark:text-teal-600">
          <strong>Días restantes:</strong>
          {{
            eventoSeleccionado.diasRestantes === 0
              ? "Hoy"
              : eventoSeleccionado.diasRestantes === 1
              ? "1 día"
              : `${eventoSeleccionado.diasRestantes} días`
          }}
        </p>
        <button
          @click="cerrarModal"
          class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-700 transition duration-300 dark:bg-teal-500 dark:hover:bg-teal-700"
        >
          Cerrar
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination, Navigation } from "swiper/modules";
import "swiper/css";
import "swiper/css/pagination";
import "swiper/css/navigation";

export default {
  components: {
    Swiper,
    SwiperSlide,
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

    onMounted(async () => {
      try {
        cargando.value = true;
        const respuesta = await fetch("/eventos.json");
        if (!respuesta.ok) {
          throw new Error("Error al obtener los eventos");
        }
        const datos = await respuesta.json();
        const hoy = new Date();
        hoy.setUTCHours(0, 0, 0, 0);
        eventos.value = datos
          .map((evento) => {
            const [year, month, day] = evento.fecha.split("-").map(Number);
            const fechaEvento = new Date(
              Date.UTC(year, month - 1, day, 0, 0, 0, 0)
            );
            const diasRestantes = Math.ceil(
              (fechaEvento - hoy) / (1000 * 60 * 60 * 24)
            );
            return {
              ...evento,
              fecha: fechaEvento,
              dia: fechaEvento.getUTCDate().toString().padStart(2, "0"),
              mes: fechaEvento.toLocaleString("es", {
                month: "long",
                timeZone: "UTC",
              }),
              diasRestantes: diasRestantes,
              diaSemana: obtenerDiaSemana(fechaEvento),
            };
          })
          .filter((evento) => evento.fecha >= hoy)
          .sort((a, b) => a.fecha - b.fecha)
          .slice(0, 10);
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

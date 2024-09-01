<template>
  <div class="grid grid-cols-1 md:grid-cols-7 gap-4">
    <!-- Evento próximo destacado -->
    <div
      class="md:col-span-4 p-[2px] rounded-lg group relative overflow-hidden"
    >
      <!-- Borde gradiente -->
      <div
        class="absolute inset-0 bg-white dark:bg-gradient-to-r from-teal-500 to-blue-500 rounded-lg animate-gradient"
      ></div>

      <!-- Contenido de la tarjeta -->
      <div
        class="dark:bg-slate-600/85 h-full p-6 rounded-lg shadow-xl relative z-10"
      >
        <div class="flex flex-col md:flex-row items-start md:items-center mb-4">
          <div
            :class="[
              'text-5xl font-bold text-black py-3 px-8 rounded-md shadow-md mr-6 mb-4 md:mb-0 relative bg-white folded-corner',
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
            <div class="w-full h-[1px] bg-gray-300 my-1"></div>
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
              class="font-bold text-xl md:text-2xl mb-2 text-gray-700 flex items-center dark:text-white"
            >
              {{ proximoEvento.titulo }}
              <svg
                v-if="proximoEvento.banner !== null"
                class="ml-2 w-5 h-5 text-gray-600"
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
            </h2>
            <p
              class="text-base md:text-lg text-gray-600 flex items-center mb-2 dark:text-white"
            >
              <i class="fas fa-calendar-alt mr-2"></i>
              {{ proximoEvento.diaSemana }}, {{ proximoEvento.hora }}
            </p>
            <p
              class="text-base md:text-lg text-gray-600 dark:text-white flex items-center mb-2"
            >
              <i
                :class="{
                  'fas fa-map-marker-alt': proximoEvento.lugar,
                  'fas fa-link': proximoEvento.link,
                }"
                class="mr-2"
              ></i>
              {{ proximoEvento.lugar || proximoEvento.link }}
            </p>
            <p
              class="text-base md:text-lg my-2 text-gray-700 line-clamp-2 dark:text-white"
            >
              {{ proximoEvento.descripcion }}
            </p>

            <!-- Banner disponible o no disponible -->
            <div v-if="proximoEvento.banner">
              <a
                href="#"
                @click.prevent="$emit('abrir-modal', proximoEvento)"
                class="text-blue-600 hover:text-blue-800 dark:text-white text-sm cursor-pointer flex items-center"
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
                @click.prevent="$emit('abrir-modal', proximoEvento)"
                class="text-gray-500 dark:text-white text-sm cursor-pointer flex items-center"
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
              class="text-base md:text-lg font-semibold text-blue-500 mt-4 dark:text-white flex items-center"
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
      </div>
    </div>

    <!-- Lista de eventos adicionales -->
    <div class="md:col-span-3 space-y-4">
      <div
        v-for="evento in eventosAdicionales"
        :key="evento.fecha"
        class="bg-white p-4 rounded-lg shadow flex items-center justify-between relative overflow-hidden"
      >
        <!-- Borde gradiente -->
        <div
          class="absolute inset-0 p-[2px] rounded-lg dark:bg-gradient-to-r from-teal-500 to-blue-500 animate-gradient"
        >
          <div class="dark:bg-slate-600/85 h-full w-full rounded-lg"></div>
        </div>

        <!-- Contenido de la tarjeta -->
        <div class="flex items-center relative z-10">
          <div
            :class="[
              'text-2xl font-bold text-black border py-2 px-4 rounded-md shadow-md mr-4 relative bg-white',
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
            <div class="w-full h-[1px] bg-gray-300 my-[0.5px]"></div>
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
            <h3 class="text-sm font-semibold flex items-center dark:text-white">
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
            <p class="text-sm text-gray-600 dark:text-white">
              {{ evento.diaSemana }}, {{ evento.hora }} -
              {{ evento.lugar }}
            </p>
          </div>
        </div>
        <button
          @click="$emit('abrir-modal', evento)"
          class="bg-blue-500 text-white px-3 py-1 rounded-lg hover:bg-blue-700 transition duration-300 text-sm relative z-10 transform hover:-translate-y-1"
        >
          Detalles
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import InfoIcono from "./InfoIcono.vue";

export default {
  components: {
    InfoIcono,
  },
  props: {
    proximoEvento: {
      type: Object,
      required: true,
    },
    eventosAdicionales: {
      type: Array,
      required: true,
    },
  },
  emits: ["abrir-modal"],
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

<template>
  <div
    v-if="evento"
    @click="cerrarSiEsFondo"
    class="fixed inset-0 backdrop-blur-sm bg-gray-900/50 flex items-center justify-center z-50"
  >
    <div
      ref="modalContent"
      @touchstart="iniciarArrastre"
      @touchmove="arrastrar"
      @touchend="finalizarArrastre"
      class="bg-white p-6 rounded-lg shadow-xl w-full sm:max-w-xl relative transition-transform duration-300 ease-out max-h-90vh overflow-y-auto"
      :style="{ transform: `translateY(${desplazamientoY}px)` }"
    >
      <!-- Indicador de arrastre (solo visible en móviles) -->
      <div
        class="w-16 h-1 bg-gray-300 rounded-full mx-auto mb-4 sm:hidden"
      ></div>

      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl sm:text-2xl font-bold flex-grow text-center">
          {{ evento.titulo }}
        </h2>
        <button
          @click="cerrar"
          class="hidden sm:block text-gray-600 hover:text-gray-800 transition duration-300"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-8 w-8"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>
      </div>
      <div class="flex flex-col md:flex-row">
        <div
          v-if="evento.banner !== null"
          class="md:w-1/2 mb-4 md:mb-0 md:mr-4 flex flex-col items-center relative"
        >
          <img
            :src="evento.banner"
            alt="Imagen del evento"
            class="w-2/3 h-auto rounded-lg cursor-pointer"
            @click="ampliarImagen"
          />
          <div
            class="absolute top-2 right-2 bg-white bg-opacity-75 rounded-full p-1 cursor-pointer"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6 text-gray-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              @click="ampliarImagen"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7"
              />
            </svg>
          </div>
        </div>
        <div
          :class="{
            'md:w-1/2': evento.banner !== null,
            'w-full': evento.banner === null,
          }"
        >
          <p class="mb-2">
            <strong>Fecha:</strong> {{ evento.dia }} de {{ evento.mes }}
          </p>
          <p class="mb-2"><strong>Hora:</strong> {{ evento.hora }}</p>
          <p class="mb-2">
            <strong>{{ evento.lugar ? "Lugar" : "Link" }}:</strong>
            {{ evento.lugar || evento.link }}
          </p>
          <p class="mb-2">
            <strong>Descripción:</strong> {{ evento.descripcion }}
          </p>
          <p class="text-red-600 dark:text-blue-600">
            <strong>Días restantes:</strong>
            {{
              evento.diasRestantes === 0
                ? "Hoy"
                : evento.diasRestantes === 1
                ? "1 día"
                : `${evento.diasRestantes} días`
            }}
          </p>
        </div>
      </div>
    </div>

    <!-- Nueva sección para la imagen ampliada -->
    <div
      v-if="imagenAmpliada"
      class="fixed inset-0 bg-black bg-opacity-75 flex items-center justify-center z-60"
      @click="cerrarImagenAmpliada"
    >
      <img
        :src="evento.banner"
        alt="Imagen ampliada del evento"
        class="max-w-90vw max-h-90vh object-contain rounded-xl"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    evento: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      imagenAmpliada: false,
      desplazamientoY: 0,
      inicioY: 0,
      umbralCierre: 100, // Píxeles que el usuario debe arrastrar para cerrar
    };
  },
  methods: {
    cerrar() {
      this.toggleBodyScroll(false);
      this.$emit("cerrar");
    },
    toggleBodyScroll(disable) {
      if (disable) {
        document.body.classList.add("overflow-hidden");
      } else {
        document.body.classList.remove("overflow-hidden");
      }
    },
    cerrarSiEsFondo(event) {
      if (event.target === event.currentTarget) {
        this.cerrar();
      }
    },
    ampliarImagen() {
      this.imagenAmpliada = true;
    },
    cerrarImagenAmpliada(event) {
      if (
        event.target === event.currentTarget ||
        event.target.tagName === "BUTTON"
      ) {
        this.imagenAmpliada = false;
      }
    },
    iniciarArrastre(event) {
      this.inicioY = event.touches[0].clientY;
    },
    arrastrar(event) {
      const currentY = event.touches[0].clientY;
      const diferencia = currentY - this.inicioY;
      if (diferencia > 0) {
        // Solo permitir arrastre hacia arriba
        this.desplazamientoY = diferencia;
      }
    },
    finalizarArrastre() {
      if (this.desplazamientoY > this.umbralCierre) {
        this.cerrar();
      } else {
        this.desplazamientoY = 0;
      }
    },
  },
  mounted() {
    this.toggleBodyScroll(true);
  },

  beforeDestroy() {
    this.toggleBodyScroll(false);
  },
};
</script>

<style scoped>
.max-w-90vw {
  max-width: 90vw;
}
.max-h-90vh {
  max-height: 90vh;
}
</style>

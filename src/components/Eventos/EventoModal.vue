<template>
  <div
    v-if="evento"
    @click="cerrarSiEsFondo"
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
  >
    <div class="bg-white p-6 rounded-lg shadow-xl max-w-xl w-full relative">
      <div class="flex justify-between items-center mb-4">
        <h2 class="text-xl sm:text-2xl font-bold">
          {{ evento.titulo }}
        </h2>
        <button
          @click="cerrar"
          class="text-gray-600 hover:text-gray-800 transition duration-300"
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
            class="absolute top-2 right-2 bg-white bg-opacity-75 rounded-full p-1"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6 text-gray-600"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
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
        class="max-w-90vw max-h-90vh object-contain"
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
    };
  },
  methods: {
    cerrar() {
      this.$emit("cerrar");
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

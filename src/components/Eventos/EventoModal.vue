<template>
  <div
    v-if="evento"
    @click="cerrarSiEsFondo"
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
  >
    <div class="bg-white p-8 rounded-lg shadow-xl max-w-xl w-full relative">
      <button
        @click="cerrar"
        class="absolute top-3 right-3 text-gray-600 hover:text-gray-800 transition duration-300"
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
      <div class="flex flex-col md:flex-row">
        <div
          v-if="evento.banner !== null"
          class="md:w-1/2 mb-4 md:mb-0 md:mr-4 flex justify-center"
        >
          <img
            :src="evento.banner"
            alt="Imagen del evento"
            class="w-2/3 h-auto rounded-lg"
          />
        </div>
        <div
          :class="{
            'md:w-1/2': evento.banner !== null,
            'w-full': evento.banner === null,
          }"
        >
          <h2 class="text-xl sm:text-2xl font-bold mb-2">
            {{ evento.titulo }}
          </h2>
          <p class="mb-2">
            <strong>Fecha:</strong> {{ evento.dia }} de
            {{ evento.mes }}
          </p>
          <p class="mb-2"><strong>Hora:</strong> {{ evento.hora }}</p>
          <p class="mb-2"><strong>Lugar:</strong> {{ evento.lugar }}</p>
          <p class="mb-2">
            <strong>Descripción:</strong>
            {{ evento.descripcion }}
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
  methods: {
    cerrar() {
      this.$emit("cerrar");
    },
    cerrarSiEsFondo(event) {
      // Cerrar solo si se hace clic en el fondo (no en el contenido del modal)
      if (event.target === event.currentTarget) {
        this.cerrar();
      }
    },
  },
};
</script>

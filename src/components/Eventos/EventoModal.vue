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
          class="md:w-1/2 mb-4 md:mb-0 md:mr-4 flex flex-col items-center"
        >
          <img
            :src="evento.banner"
            alt="Imagen del evento"
            class="w-2/3 h-auto rounded-lg cursor-pointer"
            @click="ampliarImagen"
          />
          <div class="flex mt-2">
            <button
              @click="ampliarImagen"
              class="mr-2 p-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 transition duration-300"
              title="Ampliar imagen"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
            <button
              @click="guardarImagen"
              class="p-2 bg-green-500 text-white rounded-full hover:bg-green-600 transition duration-300"
              title="Guardar imagen"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  d="M7.707 10.293a1 1 0 10-1.414 1.414l3 3a1 1 0 001.414 0l3-3a1 1 0 00-1.414-1.414L11 11.586V6h5a2 2 0 012 2v7a2 2 0 01-2 2H4a2 2 0 01-2-2V8a2 2 0 012-2h5v5.586l-1.293-1.293zM9 4a1 1 0 012 0v2H9V4z"
                />
              </svg>
            </button>
          </div>
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
    async guardarImagen() {
      try {
        // Fetch the image
        const response = await fetch(this.evento.banner);
        const blob = await response.blob();

        // Create a blob URL
        const blobUrl = URL.createObjectURL(blob);

        // Create a link element and trigger the download
        const link = document.createElement("a");
        link.href = blobUrl;
        link.download = `${this.evento.titulo}.jpg`; // You can change the extension if needed
        document.body.appendChild(link);
        link.click();

        // Clean up
        document.body.removeChild(link);
        URL.revokeObjectURL(blobUrl);
      } catch (error) {
        console.error("Error al descargar la imagen:", error);
        // You might want to show an error message to the user here
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

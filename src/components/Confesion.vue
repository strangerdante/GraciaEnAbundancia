<template>
  <div class="px-4 sm:px-14 lg:px-80 py-10 mt-20">
    <h1
      class="text-3xl text-center sm:text-left sm:text-4xl font-serif mb-4 dark:text-white"
    >
      Confesión de fe
    </h1>
    <p class="text-lg mb-4 dark:text-white">
      Descubra qué esperar al visitarnos
    </p>
    <p class="text-gray-600 mb-4 dark:text-white">
      Conozca más sobre nuestros valores y nuestra historia como iglesia
      visitando la página sobre nosotros.
    </p>
    <a
      href="/#inicio"
      class="bg-blue-500 text-white hover:bg-blue-400 px-6 py-2 rounded mb-8 transition duration-300 ease-in-out transform hover:scale-105 inline-block"
    >
      Conocenos
    </a>
    <div class="space-y-6">
      <div
        v-for="(item, index) in preguntas"
        :key="index"
        class="dark:p-0.5 dark:pt-0 dark:border-0 border-2 border-gray-200 rounded-lg bg-gray-100 dark:bg-gradient-to-tr from-blue-500 to-red-500 dark:border-slate-600 overflow-hidden"
      >
        <div
          class="h-full w-full dark:bg-slate-600/90 rounded-md transition duration-300 ease-in-out"
        >
          <!-- Barra progeso -->
          <div
            class="h-1 bg-blue-500 transition-all duration-300 ease-in-out"
            :style="{ width: item.isOpen ? '100%' : '0%' }"
          ></div>

          <button
            @click="toggleItem(index)"
            class="w-full text-left flex items-start p-4 focus:outline-none"
          >
            <span
              class="text-sm sm:text-lg font-medium dark:text-white flex-grow mr-4"
            >
              {{ item.pregunta }}
            </span>
            <div class="flex-shrink-0 w-5 mt-1">
              <svg
                :class="{ 'rotate-180': item.isOpen }"
                class="w-5 h-5 transition-transform duration-200 text-blue-500"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="currentColor"
              >
                <path
                  fill-rule="evenodd"
                  d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                  clip-rule="evenodd"
                />
              </svg>
            </div>
          </button>
          <div
            v-if="item.isOpen"
            class="p-4 transition-all duration-300 ease-in-out"
          >
            <p class="dark:text-white">{{ item.respuesta }}</p>
            <div
              v-if="item.versiculos"
              class="flex flex-wrap border-t justify-start gap-2 border-gray-300 dark:border-gray-600 p-4 mt-6"
            >
              <span
                v-for="(versiculo, vIndex) in item.versiculos"
                :key="vIndex"
                class="border-2 border-blue-600 rounded-lg text-xs sm:text-sm px-3 py-1 flex-shrink-0 text-blue-600 dark:border-blue-400 dark:text-white hover:bg-blue-600 hover:text-white transition duration-300 cursor-pointer"
              >
                {{ versiculo }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      preguntas: [
        {
          pregunta: "1. Ejemplo",
          respuesta: "Ejemplo",
          versiculos: ["versiculo", "versiculo", "versiculo"],
          isOpen: false,
        },
        {
          pregunta: "2. Ejemplo",
          respuesta: "Ejemplo",
          versiculos: ["versiculo", "versiculo", "versiculo", "versiculo"],
          isOpen: false,
        },
        {
          pregunta: "3. Ejemplo",
          respuesta: "Ejemplo",
          versiculos: ["versiculo", "versiculo", "versiculo"],
          isOpen: false,
        },
        {
          pregunta: "4. Ejemplo",
          respuesta: "Ejemplo",
          versiculos: ["versiculo", "versiculo"],
          isOpen: false,
        },
        {
          pregunta: "5. Ejemplo",
          respuesta: "Ejemplo",
          versiculos: ["versiculo", "versiculo"],
          isOpen: false,
        },
      ],
    };
  },
  methods: {
    toggleItem(index) {
      this.preguntas[index].isOpen = !this.preguntas[index].isOpen;
    },
  },
};
</script>

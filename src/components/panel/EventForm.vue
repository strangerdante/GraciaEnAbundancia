<script setup lang="ts">
import { ref, defineEmits, defineProps, watch } from "vue";

const props = defineProps({
  event: {
    type: Object,
    default: () => ({
      titulo: "",
      descripcion: "",
      textoBoton: "",
      linkBoton: "",
      image: "",
    }),
  },
  isEdit: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["submit", "cancel"]);

const formData = ref({ ...props.event });
const selectedImageOption = ref("none");
const customImageUrl = ref("");

const imageOptions = [
  { value: "none", label: "Ninguna" },
  {
    value: "biblia1",
    label: "Imagen biblia 1",
    url: "https://i.ibb.co/9Td3wVk/ben-white-W8-Qqn1-Pm-QH0-unsplash.jpg",
  },
  {
    value: "biblia2",
    label: "Imagen biblia 2",
    url: "https://i.ibb.co/KW60XdT/aaron-burden-9zs-HNt5-Opq-E-unsplash.jpg",
  },
  {
    value: "biblia3",
    label: "Imagen biblia 3",
    url: "https://i.ibb.co/b786r8G/biblia.jpg",
  },
  { value: "custom", label: "Imagen personalizada" },
];

// Set initial image option based on existing image
watch(
  () => props.event,
  (newEvent) => {
    if (newEvent.image) {
      const predefinedImage = imageOptions.find(
        (option) => option.url === newEvent.image
      );
      if (predefinedImage) {
        selectedImageOption.value = predefinedImage.value;
      } else {
        selectedImageOption.value = "custom";
        customImageUrl.value = newEvent.image;
      }
    } else {
      selectedImageOption.value = "none";
      customImageUrl.value = "";
    }
  },
  { immediate: true }
);

// Update formData.image when selection changes
watch([selectedImageOption, customImageUrl], () => {
  if (selectedImageOption.value === "custom") {
    formData.value.image = customImageUrl.value;
  } else if (selectedImageOption.value === "none") {
    formData.value.image = "";
  } else {
    const selected = imageOptions.find(
      (option) => option.value === selectedImageOption.value
    );
    formData.value.image = selected?.url || "";
  }
});

const handleSubmit = () => {
  emit("submit", formData.value);
};
</script>

<template>
  <div class="bg-white max-w-xl mx-auto">
    <form @submit.prevent="handleSubmit" class="space-y-6">
      <div>
        <label class="block text-sm font-semibold text-gray-700 mb-2"
          >Título</label
        >
        <input
          v-model="formData.titulo"
          type="text"
          placeholder="Ingrese el título"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-300"
        />
      </div>

      <div>
        <label class="block text-sm font-semibold text-gray-700 mb-2"
          >Descripción</label
        >
        <textarea
          v-model="formData.descripcion"
          rows="4"
          placeholder="Ingrese la descripción"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-300"
        ></textarea>
      </div>

      <div>
        <label class="block text-sm font-semibold text-gray-700 mb-2"
          >Imagen</label
        >
        <div class="grid grid-cols-3 gap-3">
          <div
            v-for="option in imageOptions"
            :key="option.value"
            class="flex items-center space-x-2"
          >
            <input
              type="radio"
              :id="option.value"
              v-model="selectedImageOption"
              :value="option.value"
              class="text-blue-600 focus:ring-blue-500 border-gray-300"
            />
            <label
              :for="option.value"
              class="text-sm text-gray-700 cursor-pointer"
            >
              {{ option.label }}
            </label>
          </div>
        </div>

        <!-- Preview of selected image -->
        <div v-if="formData.image" class="mt-4 flex justify-center">
          <img
            :src="formData.image"
            alt="Vista previa"
            class="h-48 w-full object-cover rounded-lg shadow-md"
          />
        </div>

        <!-- Custom image URL input -->
        <div v-if="selectedImageOption === 'custom'" class="mt-4">
          <input
            v-model="customImageUrl"
            type="text"
            placeholder="Ingrese la URL de la imagen"
            class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-300"
          />
        </div>
      </div>

      <div>
        <label class="block text-sm font-semibold text-gray-700 mb-2">
          Texto del botón
        </label>
        <input
          v-model="formData.textoBoton"
          type="text"
          placeholder="Ingrese el texto del botón"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-300"
        />
      </div>

      <div>
        <label class="block text-sm font-semibold text-gray-700 mb-2">
          Link del botón
        </label>
        <input
          v-model="formData.linkBoton"
          type="text"
          placeholder="Ingrese el link del botón"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-300"
        />
      </div>

      <div class="flex justify-end space-x-4">
        <button
          type="button"
          @click="$emit('cancel')"
          class="px-4 py-2 text-sm font-medium text-gray-700 bg-white hover:bg-gray-50 border border-gray-300 rounded-md transition duration-300 ease-in-out"
        >
          Cancelar
        </button>
        <button
          type="submit"
          class="px-4 py-2 text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 rounded-md shadow-md transition duration-300 ease-in-out focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
        >
          {{ isEdit ? "Actualizar" : "Crear" }}
        </button>
      </div>
    </form>
  </div>
</template>

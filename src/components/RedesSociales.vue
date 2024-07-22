<template>
  <section class="bg-blue-100 py-8">
    <div class="container mx-auto px-4">
      <h2 class="text-2xl font-bold text-center text-gray-800 mb-6">
        Síguenos en nuestras redes sociales
      </h2>
      <div class="flex justify-center space-x-6">
        <a
          v-for="(social, index) in socialMedia"
          :key="index"
          :href="social.url"
          target="_blank"
          rel="noopener noreferrer"
          class="text-gray-700 hover:text-teal-500 transition-colors duration-300"
        >
          <i :class="social.icon" class="text-3xl"></i>
          <span class="sr-only">{{ social.name }}</span>
        </a>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      socialMedia: [
        {
          name: "Instagram",
          url: "https://instagram.com/graciaenabundancia_ib",
          icon: "fab fa-instagram",
        },
        {
          name: "YouTube",
          url: " https://youtube.com/@graciaenabundancia_ib",
          icon: "fab fa-youtube",
        },
        {
          name: "Facebook",
          url: "https://www.facebook.com/GraciaEnAbundanciaIB",
          icon: "fab fa-facebook",
        },
      ],
    };
  },
};
</script>

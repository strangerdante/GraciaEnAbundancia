redes sociales
<template>
  <section class="relative overflow-hidden">
    <!-- Fondo con efecto parallax -->
    <div
      class="absolute inset-0 bg-cover bg-center bg-fixed"
      :style="{ backgroundImage: `url(${backgroundImage})` }"
    ></div>

    <!-- Contenido -->
    <div class="relative z-10 bg-blue-100 bg-opacity-80 py-16">
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
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      backgroundImage:
        "https://images.unsplash.com/photo-1504052434569-70ad5836ab65?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
      socialMedia: [
        {
          name: "Instagram",
          url: "https://instagram.com/graciaenabundancia_ib",
          icon: "fab fa-instagram",
        },
        {
          name: "YouTube",
          url: "https://youtube.com/@graciaenabundancia_ib",
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

<template>
  <div>
    <MenuInicio />
    <Bienvenida />
    <SobreNosotros />

    <CarruselAnuncios />
    <Calendario />
    <Pastor />
    <Servicios />
    <Ministerios />
    <Ubicacion />
    <PieDePagina />
    <BotonTop />
  </div>
</template>

<script>
import PieDePagina from "./PieDePagina.vue";
import Ubicacion from "./Ubicacion.vue";
import Servicios from "./Servicios.vue";
import Pastor from "./Pastor.vue";
import Bienvenida from "./Bienvenida.vue";
import MenuInicio from "./MenuInicio.vue";
import Calendario from "./Calendario.vue";
import CarruselAnuncios from "./CarruselAnuncios.vue";
import Ministerios from "./Ministerios.vue";
import BotonTop from "./BotonTop.vue";
import SobreNosotros from "./SobreNosotros.vue";

export default {
  components: {
    PieDePagina,
    Ubicacion,
    Ministerios,
    Servicios,
    Pastor,
    CarruselAnuncios,
    Bienvenida,
    MenuInicio,
    SobreNosotros,
    Calendario,
    BotonTop,
  },
  data() {
    return {};
  },
  computed: {},
  methods: {},
  mounted() {
    // RefTagger
    window.refTagger = {
      settings: {
        bibleVersion: "RVR60",
        tooltipStyle: "dark",
        libronixBibleVersion: "DEFAULT",
        addLogosLink: false,
        appendIconToLibLinks: false,
        libronixLinkIcon: "dark",
        noSearchClassNames: ["commentList"],
        useTooltip: true,
        noSearchTagNames: ["h1", "h2", "h3"],
        linksOpenNewWindow: true,
        convertHyperlinks: false,
        caseInsensitive: false,
        tagChapters: true,
      },
    };

    // Cargar RefTagger
    const script = document.createElement("script");
    script.src = "https://api.reftagger.com/v2/reftagger.es.js";
    script.async = true;
    document.body.appendChild(script);
  },
};
</script>

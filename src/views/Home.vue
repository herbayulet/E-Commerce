<template>
  <div class="home">
    <HeaderHyung />
    <HeroHyung />
    <WomenHyung />
    <InstaHyung />
    <PartnerHyung />
    <FooterHyung />
  </div>
</template>

<script>
// component dari header supaya bisa jalan
import HeaderHyung from "@/components/HeaderHyung.vue";
//carousel ini ditaro di home karena memang slider nye untuk halaman/pages home
import HeroHyung from "@/components/HeroHyung.vue";
// Ini buat slider product kek switer sama tas
import WomenHyung from "@/components/WomenHyung.vue";
// ini untuk section instagram/gallery dari toko nye
import InstaHyung from "@/components/InstaHyung.vue";
// ini untuk logo dari partner toko nye;
import PartnerHyung from "@/components/PartnerHyung.vue";
// this is last fucking section footer
import FooterHyung from "@/components/FooterHyung.vue";

export default {
  name: 'Home',
  components: {
    HeaderHyung,
    HeroHyung,
    WomenHyung,
    InstaHyung,
    PartnerHyung,
    FooterHyung
  }
}
</script>

<template>
  <div class="product">
    <HeaderHyung />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i>Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="foto" alt="" />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                  <carousel :dots="false" :nav="false" class="product-thumbs-track ps-slider">
                    
                    <div
                    v-for="ss in productDetails.galleries"
                    :key="ss.id" 
                    class="pt" 
                    @click="gantiFoto(ss.photo)" 
                    :class="ss.photo == foto ? 'active' : '' ">
                      <img :src="ss.photo" alt="" />
                    </div>

                  </carousel>
                </div>
              </div> 
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description">
                    </p>
                    <h4>${{productDetails.price}}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/shopping">
                    <a @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add to Cart</a>
                   </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->
    <RelationHyung />
    <FooterHyung />
  </div>
</template>

<script>
// component dari header supaya bisa jalan
import HeaderHyung from "@/components/HeaderHyung.vue";
// this is last fucking section footer
import FooterHyung from "@/components/FooterHyung.vue";
// Ini buat slider product yang tipe nye berelasi kek switer sama tas
import RelationHyung from "@/components/RelationHyung.vue";

// ini buat aktifin carousel nye
import carousel from "vue-owl-carousel";

// ini buat make axios nya
import axios from "axios"

export default {
  name: "Product",
  components: {
    HeaderHyung,
    FooterHyung,
    RelationHyung,
    carousel
  },

  data() {
    return {
      foto:"",
      productDetails: [],
      keranjangUser: []
    };
  },
  // ini untuk mengganti foto yang kita klik 
  methods: {
      gantiFoto(urlImage) {
          this.foto = urlImage;
      // eslint-disable-next-line no-console
      },
       setDataFoto(data) {
        // mengganti object productDetails make data dari API
        this.productDetails = data;
        // mengganti foto make data dari API
        this.foto = data.galleries[0].photo;
      },
      // untuk mengecek produk yang kita klik ini sesuai atau tidak dengan database nya based on id dan nama
      saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {

        var productStored = {
          "id": idProduct,
          "name": nameProduct,
          "price": priceProduct,
          "photo": photoProduct
        }
        // push disini untuk memasukan item nya tersebut ke dalam variable productStored
        this.keranjangUser.push(productStored)
        const parsed = JSON.stringify(this.keranjangUser);
        localStorage.setItem('keranjangUser', parsed);
      }
  },
  // untuk memvalidasi apakah benar produk yang kita masukan berdasarkan data dari API
  mounted() {
        if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch(e) {
        localStorage.removeItem('keranjangUser');
      }
    }
    // ini adalah untuk integrasi data API nya
        axios
        .get("http://127.0.0.1:8001/api/products", {
          params: {
             id: this.$route.params.id
          }
        })
        .then(res => (this.setDataFoto(res.data.data)))
        // eslint-disable-next-line no-console
        .catch(err => console.log(err));
    }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 12px;
}
</style>

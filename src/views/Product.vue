<template>
  <div class="product">
    <HeaderShayna />
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
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
                  <img class="product-big-img" :src="gambar_default" alt="" />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                  <carousel
                    :dots="false"
                    :loop="true"
                    :nav="false"
                    class="product-thumbs-track ps-slider"
                  >


                    <div
                    v-for="ss in productDetails.galleries"
                    :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo == gambar_default ? 'active' : ''"
                    >
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
                  
                    <p>
                     {{ productDetails.description }}
                    </p>
                    <h4>${{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                      <a href="shopping-cart.html" class="primary-btn pd-cart"
                        >Add To Cart</a
                      >
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <RelatedShayna />
    <FooterShayna />
  </div>
</template>

<script>
import HeaderShayna from "@/components/HeaderShayna.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import RelatedShayna from "@/components/RelatedShayna.vue";
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "product",
  components: {
    HeaderShayna,
    FooterShayna,
    RelatedShayna,
    carousel,
  },
  data() {
    return {
      gambar_default: "img/mickey1.jpg",
      thumbs: [
        "img/mickey1.jpg",
        "img/mickey2.jpg",
        "img/mickey3.jpg",
        "img/mickey4.jpg",
      ],
      productDetails: []
    }
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },
    setDataPicture(data){
        this.productDetails = data;
        this.gambar_default = data.galleries[0].photo
      
  }
  },
  
  mounted() {
    axios
      .get("http://shayna-backend.belajarkoding.com/api/products", {
        params: {id:this.$route.params.id}
      })
      .then(res => (this.setDataPicture(res.data.data)))
      .catch((err) => console.log(err));
  },
};
</script>

<style>
.product-thumbs .pt {
  margin-right: 14px;
}
</style>

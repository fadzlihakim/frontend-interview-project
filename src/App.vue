<script>
import { Navigation } from "swiper/modules";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/navigation";
export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  setup() {
    const onSwiper = (swiper) => {
      // console.log(swiper);
    };
    const onSlideChange = () => {
      // console.log("slide change");
    };
    return {
      onSwiper,
      onSlideChange,
      modules: [Navigation],
    };
  },
  data() {
    return {
      counter: 0,
      status: "Hurry Limited Stock",
      brand: "Samsung",
      description: "Samsung Galaxy S23 Plus 5G",
      price: 880.0,
      sim: "Dual Sim",
      condition: "Like New",
      colour: "Lavender",
      network: "Unlocked",
      storage: "512GB",
      image: [
        {
          tag: "front-large",
          image: "assets/product-image-front-large.jpeg",
          thumbnail: "assets/product-image-front-thumbnail.jpeg",
        },
        {
          tag: "image-large",
          image: "assets/product-image-large.jpeg",
          thumbnail: "assets/product-image-thumbnail.jpeg",
        },
        {
          tag: "back-large",
          image: "assets/product-image-back-large.jpeg",
          thumbnail: "assets/product-image-back-thumbnail.jpeg",
        },
      ],
      videos: [
        { tag: "double-tap", video: "videos/double-tap-zoom-mobile.mp4" },
        { tag: "drag-swipe", video: "videos/drag-swipe-in-mobile.mp4" },
        { tag: "single-click", video: "videos/single-click-zoom-desktop.mp4" },
      ],
    };
  },
};
</script>

<template>
  <header>
    <div class="container">
      <img class="headerImage" src="/assets/tbps-logo.webp" alt="" />
    </div>
  </header>
  <section>
    <div class="container" id="content">
      <div class="item">
        <div class="main">
          <swiper
            :modules="modules"
            :slides-per-view="1"
            :space-between="50"
            navigation
            @swiper="onSwiper"
            @slideChange="onSlideChange">
            <swiper-slide v-for="item in image">
              <img class="itemSwiper" v-bind:src="item.image" />
            </swiper-slide>
          </swiper>
          <div class="thumbnail">
            <img
              class="thumbnailImage"
              v-for="item in image"
              v-bind:src="item.thumbnail" />
            <video
              class="thumbnailVideo"
              v-for="item in videos"
              v-bind:src="item.video"></video>
          </div>
        </div>
        <div class="description" id="describe">
          <label
            >{{ brand }} <span class="status">{{ status }}</span></label
          >
          <h2>{{ description }}</h2>
          <h3>&#163;{{ price }}</h3>
          <hr />
          <label>Sim: {{ sim }}</label>
          <label>Condition: {{ condition }}</label>
          <label>Colour: {{ colour }}</label>
          <label>Network: {{ network }}</label>
          <label>Storage: {{ storage }}</label>
          <hr />
          <div class="counter">
            <button class="counterButton" @click="counter--">-</button>
            <label id="counter">{{ counter }}</label>
            <button class="counterButton" @click="counter++">+</button>
          </div>
          <button type="button" class="button">Add to Basket</button>
        </div>
      </div>
    </div>
  </section>
</template>
<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>

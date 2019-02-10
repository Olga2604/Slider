<template>
  <div class="image-slider">
    <div class="image-slider__container"
         v-for="number in [currentNumber]"
         :key="number">
      <div :style="{ 'background': `url(${currentSlide}) no-repeat`,
                     'background-size': 'cover',
                     'background-position': 'center center' }"
           class="image-slider__slide"
           @mouseover="stopSliderRotations"
           @mouseout="startSliderRotations"
           v-if="getContentSlider && getContentSlider.length > 0"
           v-html="currentSlide.content">
      </div>
    </div>
    <div class="image-slider__navigation" v-if="dotNavigation === true">
      <div class="image-slider__navigation__dot"
           v-for="(dot, index) in getCategoryContentSlider"
           :key="index"
           @click="goToSlide(index)">
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';


export default {
  props: ['dotNav', 'arrowNav'],
  data() {
    return {
      currentNumber: 0,
      timer: null,
      dotNavigation: this.dotNav,
      arrowNavigation: this.arrowNav,
    };
  },
  created() {
    this.loadContent();
  },
  mounted() {
    this.startSliderRotations();
  },
  computed: {
    ...mapGetters(['getContentSlider']),
    currentSlide() {
      return this.getContentSlider[Math.abs(this.currentNumber)
      % this.getContentSlider.length];
    },
  },
  methods: {
    ...mapActions(['loadContent']),
    startSliderRotations() {
      this.timer = setInterval(this.getNextSlider, 3000);
    },
    stopSliderRotations() {
      clearTimeout(this.timer);
      this.timer = null;
    },
    getNextSlider() {
      this.currentNumber += 1;
    },
    getPreviousSlider() {
      this.currentNumber -= 1;
    },
    goToSlide(navIndex) {
      this.currentNumber = navIndex;
    },
  },
};
</script>

<style lang="scss" scoped>
  .image-slider {
    max-height: 600px;
    position: relative;
    width: 100%;
    background: #0a0a0a;
    &__container {
      width: 100%;
    }
    &__slide {
      height: 500px;
      padding: 50px;
      text-align: left;
    }
    &__navigation {
      width: 100%;
      position: absolute;
      top: 80%;
      left: 80%;
      &__dot {
        display: inline-block;
        background: #ffffff;
        opacity: 0.5;
        width: 20px;
        height: 20px;
        border-radius: 50%;
        cursor: pointer;
        margin: 5px;
      }
      &__dot:hover {
        background: #4a67b2;
        opacity: 1;
        box-shadow: inset 0 0 0 4px #ffffff;
      }
    }
  }
  .fade-enter-active, .fade-leave-active {
    transition: all 0.8s ease;
    overflow: hidden;
    visibility: visible;
    opacity: 1;
    position: absolute;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
    visibility: hidden;
  }
  /deep/ >>> h2 {
    font-size: 50px;
    font-weight: 200;
    color: #fbe412;
  }
  /deep/ >>> p {
    margin-top: 20px;
    color: #ffffff;
  }
  /deep/ >>> p > a {
    background: #fbe412;
    padding: 10px 40px;
    box-shadow: 0px 9px 33.25px 1.75px rgba(0, 0, 0, 0.33);
    border-radius: 22px;
    color: #0a0a0a;
  }
  @media (max-width: 767px) {
    .image-slider {
      &__slide {
        text-align: center;
      }
      &__navigation {
        text-align: center;
        margin-top: 35px;
        left: 0;
      }
    }
  }
</style>

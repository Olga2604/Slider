<template>
  <some-component />
  <image-slider :dotNav="true /> //pass a prop which tells to the slider component to use dot navigation
</template>

<script>
import SomeComponent from '../blocks/SomeElements';
import ImageSlider from '../elements/ImageSlider';

export default {
  components: {
    ImageSlider,
    SomeComponent,
  },
};
</script>

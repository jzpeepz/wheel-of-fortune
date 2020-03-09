<template>
  <div class="home">
    <WheelBoard></WheelBoard>
  </div>
</template>

<script>
// @ is an alias to /src
import WheelBoard from '@/components/WheelBoard.vue';

export default {
  name: 'Home',
  components: {
    WheelBoard,
  },
};
</script>

<template>
  <main>
    <Timer text="Get this awesome Bundle 50% off!" end_text="The offer has ended" :month="4" :day="23" :hour="13" :minute="27" :second="0" />
  </main>
</template>

<script>
import Timer from "./Timer";
export default {
  components: {
    Timer,
  },
};
</script>

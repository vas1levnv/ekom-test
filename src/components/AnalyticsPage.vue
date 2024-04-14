<script>
import {mapState} from "vuex";

export default {
  name: "AnalyticsPage",
  computed: {
    ...mapState(['visits']),
  }
}
</script>

<template>
  <div>
    {{ visits }}

  </div>
</template>

<style scoped>

</style>
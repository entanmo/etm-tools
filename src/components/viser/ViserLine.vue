<template>
  <div class="chart-content">
    <v-chart :force-fit="false"
             :height="vdata.height"
             :width="vdata.width"
             :data="vdata.data"
             :scale="vdata.scale"
             :padding="{top: 15, right: 50, bottom: 85, left: 50 }">
      <v-tooltip />
      <v-axis />
      <v-line :position="vdata.axis[0].key+'*'+vdata.axis[1].key"
              shape="hv" />
    </v-chart>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    vdata: Object
  }
};
</script>

<style scoped>
</style>

<template>
  <div class="ganttChart-container">
    <Chart />
    <div class="gantt-footer">
      <Detail class="detail" />
      <Write class="write" />
    </div>
  </div>
</template>

<script>
import Chart from "../../../component/acess/projectDetail/ganttChart/Chart.vue";
import Detail from "../../../component/acess/projectDetail/ganttChart/Detail.vue";
import Write from "../../../component/acess/projectDetail/ganttChart/Write.vue";

export default {
  components: {
    Chart,
    Detail,
    Write,
  },
};
</script>

<style scoped>
.ganttChart-container {
  padding: 20px;
}

.gantt-footer {
  display: flex;
  color: white;
}

</style>
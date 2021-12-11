<template>
  <div class="chart-container">
    <ul class="chart-date">
      <li v-for="day in date" :key="day">
        {{ day }}
      </li>
    </ul>
    <ul class="chart-bars">
      <li v-for="task in ganttData.tasks" :key="task">
        <span
          :style="{
            background:
              `linear-gradient` +
              '(' +
              '90deg' +
              ',' +
              `${task.color} 0` +
              ',' +
              ` #fff ${task.progress}%` +
              ')',
          }"
        >
        </span>
        <p>{{ task.title }}</p>
      </li>
    </ul>
    <div class="todayLine" :style="{ left : `${todayLineOffset}px` }"></div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "chart",
  data() {
    return {
      ganttData: this.$store.state.gantt.chart,
      date: [],
      month: "",
      todayLineOffset : 0,
    };
  },
  created() {
    this.dateRender();
  },
  mounted() {
    this.createChart();
  },
  updated(){
    this.createChart();
  },
  methods: {
    dateRender() {
      let today = moment().format("YYYY-MM-DD").split("-");

      let lastDay = new Date(today[0], today[1], 0).getDate();

      for (let day = 1; day < lastDay + 1; day++) {
        if (day < 10) {
          day = `0${day}`;
        }

        this.date.push(`${day}`);
      }
    },
    createChart() {
      let today = moment().format("YYYY-MM-DD").split("-");

      this.year = today[0];
      this.month = today[1];

      let days = document.querySelectorAll(".chart-date li");
      let tasks = document.querySelectorAll(".chart-bars li");
      days = Array.from(days);
      tasks = Array.from(tasks);

      let left = 0,
        width = 0,
        f_arr = [];

      f_arr = days.filter((day) => day.textContent === today[2]);

      this.todayLineOffset = f_arr[0].offsetLeft + 30;

      tasks.forEach((el, index) => {

        let start = this.ganttData.tasks[index].start;

        f_arr = days.filter((day) => day.textContent === start);
        left = f_arr[0].offsetLeft;

        let end = this.ganttData.tasks[index].end;

        f_arr = days.filter((day) => day.textContent === end);
        width = f_arr[0].offsetLeft + f_arr[0].offsetWidth - left;

        el.style.left = `${left}px`;
        el.style.width = `${width}px`;

        el.style.opacity = 1;
      });
    },
  },
};
</script>

<style scoped>
.chart-container {
  color: white;
  height: calc(60vh - 70px);
  border-radius: 25px;
  background: #2C2F3B;
  padding: 20px;
  margin: 20px;
  overflow-y: scroll;
  overflow-x: hidden;
}
.chart-date {
  display: flex;
  margin: 0 0 20px 0;
  font-weight: bold;
  font-size: 1.2rem;
}

.chart-date li {
  flex: 1;
  min-width: 19px;
  text-align: center;
}

.chart-date li:not(:last-child) {
  position: relative;
}
.chart-date li:not(:last-child):before {
  position: absolute;
  content: "";
  right: 0;
  height: 100%;
  border-right: 1px solid #fff;
}

.chart-bars li {
  z-index: 5;
  position: relative;
  color: black;
  margin-bottom: 15px;
  font-size: 16px;
  border-radius: 20px;
  padding: 10px 20px;
  opacity: 1;
  height: 30px;
  background: #eee;
  transition: all 0.6s linear 0.2s;
  overflow: hidden;
  display: flex;
  -webkit-filter: drop-shadow(0px 10px 10px rgba(10, 10, 10, 0.8));
}

.chart-bars li span {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.chart-bars li p {
  position: absolute;
  top: 15%;
  margin-left: 15px;
  left: 0;
}

.todayLine{
  top : 0;
  position: absolute;
  height: 100%;
  border-right: 1px solid red;

}

::-webkit-scrollbar {
  width: 0px;
}
</style>
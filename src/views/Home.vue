<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <ve-line :data="chartData"></ve-line>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';

export default {
  name: 'Home',
  data() {
    return {
      chartData: {
        columns: ['日期', '溫度', '濕度'],
        rows: [],
      },
    };
  },
  methods: {
    timeStampconver(timeStamp) {
      // const unix_timestamp = 1549312452;
      // Create a new JavaScript Date object based on the timestamp
      // multiplied by 1000 so that the argument is in milliseconds, not seconds.
      const date = new Date(timeStamp * 1000);
      // Hours part from the timestamp
      const hours = date.getHours();
      // Minutes part from the timestamp
      const minutes = `0${date.getMinutes()}`;
      // Seconds part from the timestamp
      const seconds = `0${date.getSeconds()}`;

      // Will display time in 10:30:23 format
      const formattedTime = `${hours}:${minutes.substr(-2)}:${seconds.substr(-2)}`;
      console.log(formattedTime);
      return formattedTime;
    },
    getData() {
      const filename = 1599788224343;
      fetch(`/data/${filename}`)
        .then((r) => r.json())
        .then((json) => {
          const levels = json;
          let temp = { 日期: `${this.timeStampconver(filename)}`, 溫度: levels.bT / 10, 濕度: levels.bRH / 10 };
          this.chartData.rows.push(temp);
          let lastT = levels.bT;
          let lastRH = levels.bRH;
          // let timestamp = filename;
          for (let i = 0; i <= 90; i += 1) {
            console.log(levels.dT.charCodeAt(i));
            lastT += 77 - levels.dT.charCodeAt(i);
            lastRH += 77 - levels.dRH.charCodeAt(i);
            // timestamp += filename;
            temp = { 日期: `${this.timeStampconver(filename + i * 10)}`, 溫度: (lastT) / 10, 濕度: (lastRH) / 10 };
            this.chartData.rows.push(temp);
          }
        },
        (response) => {
          console.log('Error loading json:', response);
        });

      console.log(this.chartData);
    },
  },
  components: {
    HelloWorld,
  },
  beforeMount() {
    this.getData();
  }
  ,
};
</script>

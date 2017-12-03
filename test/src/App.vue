<template>
  <div id="app">
    <div class="dates"><h4>Start Date: </h4>{{dates.startDate}}</div>
    <div class="dates margin-bottom"><h4>End Date: </h4>{{dates.endDate}}</div>
    <vue-daterange-picker 
      double 
      start-date="06/10/2017" 
      end-date="06/10/2018"
      @get-dates="getDates"
    />
  </div>
</template>

<script>
import VueDaterangePicker from 'vue-daterange-picker';

export default {
  name: 'app',
  components: {
    VueDaterangePicker
  },
  data() {
    return {
      dates: {}
    };
  },
  methods: {
    getDates(dates) {
      this.dates = dates;
    }
  }
}
</script>

<style>
  #app {
    font-family: Arial, 'sans-serif';
    font-size: 14px;
    color: 333;
  }

  .margin-bottom {
    margin-bottom: 20px;
  }

  .dates {
    display: flex;
    align-items: center;
    justify-content: flex-start;
  }

  .dates h4 {
    padding-right: 10px;
    margin: 10px 0;
  }
</style>

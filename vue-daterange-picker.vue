<template>
  <div class="daterange-picker" v-on-clickaway="closeCal" @keyup="close">
    <div class="inputs-container">
      <img src="./assets/calendar.svg" class="calendar" alt="calendar">
      <input type="text" 
              maxlength="10" 
              ref="startDateInput" 
              class="start-date" 
              placeholder="MM/DD/YY" 
              v-model="startDateLabel"
              @click.stop="handleStartDateClick">
      <div class="divider">&mdash;</div>
      <input type="text"
             maxlength="10" 
             ref="endDateInput" 
             class="end-date" 
             placeholder="MM/DD/YY" 
             v-model="endDateLabel"
             @click.stop="handleEndDateClick">
    </div>

    <div class="calendar-holder" v-show="showCalendar">
      <div class="pointer" ref="pointer"></div>
      <div class="arrow-left" ref="leftArrow" :class="{disabled: canSwitchNextMonth}" @click="gotoPrevMonth">&lsaquo;</div>
      <div class="arrow-right" ref="rightArrow" :class="{disabled: canSwitchPrevMonth}" @click="gotoNextMonth">&rsaquo;</div>

      <div class="left-calendar">
        <div class="title">{{leftMonthTitle}}</div>
        <table>
          <thead>
            <tr>
              <td v-for="(day, index) of moment.weekdaysMin()" :key="index">{{day}}</td>
            </tr>
          </thead>
          <tbody>
            <template v-for="weeks in leftCal">
              <tr>
                <td v-for="(day, index) in weeks" 
                    :key="index"
                    @click.stop="dayClick(day)"
                    @mouseover="getPotentialDate(day)"
                    :class="createDayClass(day)" >
                    {{day && day.date()}}
                </td>
              </tr>
            </template>
          </tbody>
        </table>
      </div>

      <div v-if="double" class="right-calendar">
        <div class="title">{{rightMonthTitle}}</div>
        <table>
          <thead>
            <tr>
              <td v-for="(day, index) of moment.weekdaysMin()" :key="index">{{day}}</td>
            </tr>
          </thead>
          <tbody>
            <template v-for="weeks in rightCal">
              <tr>
                <td v-for="(day, index) in weeks"
                    :key="index" 
                    @click.stop="dayClick(day)"
                    @mouseover="getPotentialDate(day)"
                    :class="createDayClass(day)">
                    {{day && day.date()}}
                </td>
              </tr>
            </template>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import { directive as onClickaway } from 'vue-clickaway';
  const moment = require('moment');

  export default {
    name: 'vue-daterange-picker',

    directives: {
      onClickaway: onClickaway,
    },

    props: {
      startDate: { type: String, default: null },
      endDate: { type: String, default: null },
      double: { type: Boolean, default: false },
      format: { type: String, default: 'MM/DD/YYYY' },
      titleFormat: { type: String, default: 'MMMM Y' },
      datesFormat: { type: String, default: 'M/D/YYYY' }
    },

    data () {
      return {
        moment,
        currentMonth: moment(this.startDate, this.datesFormat, true).startOf('month') || moment().local().startOf('month'),
        montNow: moment().local().startOf('month'),
        today: moment().local().startOf('day'),
        startDateLabel: null,
        endDateLabel: null,
        selectedStartDate: null,
        selectedEndDate: null,
        dateToInput: 'start',
        showCalendar: false,
        clicksCount: 0,
        potentialStartDate: null,
        potentialEndDate: null
      }
    },

    computed: {
      startDateAsMoment() {
        return this.moment(this.startDate, this.datesFormat, true).startOf('day');
      },

      endDateAsMoment() {
        return this.moment(this.endDate, this.datesFormat, true).startOf('day');
      },

      nextMonth() {
        return this.moment(this.currentMonth).add(1, 'month');
      },

      leftCal() {
        return this.buildCalendar(this.currentMonth);
      },

      rightCal() {
        return this.buildCalendar(this.nextMonth);
      },

      leftMonthTitle() {
        return this.currentMonth.format(this.titleFormat);
      },

      rightMonthTitle() {
        return this.nextMonth.format(this.titleFormat);
      },

      canSwitchNextMonth() {
        return this.currentMonth.isSame(this.moment(this.startDate), 'month');
      },

      canSwitchPrevMonth() {
        if (this.double) {
          return this.currentMonth.isSame(this.moment(this.endDate).subtract(1, 'month'), 'month');
        } else {
          return this.currentMonth.isSame(this.moment(this.endDate), 'month');
        }
      }
    },

    methods: {
      buildCalendar(month) {
        let tempCalendar = [];
        let calendar = [];

        for (let i = 0; i < month.daysInMonth(); i++ ) {
          tempCalendar.push(this.moment(month).date(i + 1));
        }

        let dayIndex = tempCalendar[0].day();
        for (let i = 0; i < dayIndex; i++) {
          tempCalendar.unshift(null);
        }

        return calendar = this.splitChunks(tempCalendar, 7);
      },

      splitChunks(arr, number) {
        let newArr = [];
        for (let i = 0; i < arr.length; i += number) {
          newArr.push(arr.slice(i, i + number));
        }
        return newArr;
      },

       getPotentialDate(day) {
        if (this.startDateLabel && !this.endDateLabel) {
          this.potentialEndDate = day;
        }
        if (day.isBefore(this.moment(this.startDateLabel)) && this.endDateLabel) {
          this.potentialStartDate = day;
          this.handleStartDateClick();
        } else {
          this.potentialStartDate = this.startDateLabel;
        }
        if (day.isAfter(this.moment(this.endDateLabel)) && this.startDateLabel) {
          this.potentialEndDate = day;
          this.handleEndDateClick();
        } else {
          this.potentialEndDate = this.endDateLabel;
        }
      },

      validateDate(date) {
        return date.isValid() && date.isBetween(this.startDateAsMoment, this.endDateAsMoment, null, '[]');
      },

      gotoPrevMonth() {
        this.currentMonth = this.moment(this.currentMonth.subtract(1, 'month'));
      },

      gotoNextMonth() {
        this.currentMonth = this.moment(this.currentMonth.add(1, 'month'));
      },

      dayClick(day) {
        if (!day) return;
        if (this.dateToInput === 'start') {
          this.startDateLabel = day.format(this.format);
          this.selectedStartDate = day;
          this.$refs.endDateInput.focus();
          if (!this.ignoreWatch) {
            this.handleEndDateClick();
          }
          return;
        }
        if (this.dateToInput === 'end') {
          this.endDateLabel = day.format(this.format);
          this.selectedEndDate = day;
          this.$refs.startDateInput.focus();
          if (!this.ignoreWatch) {
            this.handleStartDateClick();
          }
          return;
        }
      },

      checkDatesOrder() {
        if (this.endDateLabel && moment(this.endDateLabel).isBefore(moment(this.startDateLabel))) {
          const endDate = this.endDateLabel;
          const startDate = this.startDateLabel;
          const startDateSelected = this.selectedStartDate;
          const endDateSelected = this.selectedEndDate;
          this.endDateLabel = startDate;
          this.startDateLabel = endDate;
          this.selectedStartDate = endDateSelected;
          this.selectedEndDate = startDateSelected;
        }
      },

      handleStartDateClick() {
        this.dateToInput = 'start';
        this.$refs.pointer.style.left = '90px';
        this.$refs.startDateInput.select();      
        this.showCalendar = true;
      },

      handleEndDateClick() {
        this.dateToInput = 'end';
        this.$refs.pointer.style.left = '230px';
        this.$refs.endDateInput.select();     
        this.showCalendar = true;
      },

      closeCal() {
        this.$refs.endDateInput.blur();
        this.$refs.startDateInput.blur();
        if (this.selectedStartDate) this.startDateLabel = this.selectedStartDate.format(this.format);
        if (this.selectedEndDate) this.endDateLabel = this.selectedEndDate.format(this.format);
        this.potentialStartDate = this.startDateLabel;
        this.potentialEndDate = this.endDateLabel;
        this.$refs.pointer.style.left = '90px';
        let data = {
          startDate: this.moment(this.selectedStartDate).format(),
          endDate: this.moment(this.selectedEndDate).format()
        };

        if (this.selectedStartDate && this.selectedEndDate && this.showCalendar) this.$emit('get-dates', data);
        this.showCalendar = false;
      },

      createDayClass(day) {
        if (!day) return { nil: true };
        return {
          nil: day.isBefore(this.startDate, 'day') || day.isAfter(this.endDate, 'day'),
          current: day.isSame(this.today),
          selected: day.isSame(this.selectedStartDate) || day.isSame(this.selectedEndDate),
          potential: day.isBetween(this.potentialStartDate, this.selectedStartDate, 'day')
                    || day.isBetween(this.selectedEndDate, this.potentialEndDate, 'day'),
          ranged: day.isBetween(this.selectedStartDate, this.selectedEndDate, 'day')
                  || day.isBetween(this.selectedEndDate, this.selectedStartDate, 'day')
        };
      },
      
      close(e) {
        if (e.code === 'Escape' || e.keyCode === 27) {
          this.closeCal();
        }
      }
    }
  }
</script>

<style scoped>
  .daterange-picker {
    position: relative;
    display: inline-flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .inputs-container {
    display: flex;
    padding: 3px 10px;
    justify-content: space-between;
    align-items: center;
    border-radius: 4px;
    border: 1px solid #ccc;
    box-sizing: border-box;
  }

  .inputs-container .calendar {
    height: 30px;
    margin-right: 10px;
  }

  .inputs-container .start-date,
  .inputs-container .end-date {
    border: none;
    text-align: center;
    padding: 5px 10px;
    border-radius: 2px;
    max-width: 100px;
    box-sizing: border-box;
    outline: none;
    font-size: 14px;
    color: #555;
    font-weight: 600;
    box-sizing: border-box;
  }

  .start-date:focus,
  .end-date:focus {
    background: #eaeaea;
  }

  .inputs-container .divider {
    color: #ccc;
    margin: 0 10px;
  }

  .calendar {
    background: transparent;
  }

  .calendar-holder {
    position: relative;
    top: 15px;
    left: 0;
    display: flex;
    flex-shrink: 0;
    padding: 15px;
    border-radius: 5px;
    background: #fff;
    box-shadow: 0 3px 8px 1px rgba(0,0,0,0.26);
    box-sizing: border-box;
  }

  .pointer {
    content: '';
    display: block;
    width: 14px;
    height: 14px;
    background: #fff;
    position: absolute;
    top: -8px;
    left: 90px;
    transform: rotate(45deg);
    border-left: 1px solid #eaeaea;
    border-top: 1px solid #eaeaea;
    transition: left .2s ease-in-out;
  }

  .arrow-left,
  .arrow-right {
    position: absolute;
    font-size: 26px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 25px;
    height: 25px;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .arrow-left,
  .arrow-right {
    cursor: pointer;
    user-select: none;
    top: 15px;
    left: 20px;
    color: #41B883;
  }

  .arrow-right {
    left: auto;
    right: 20px;
  }

  .arrow-left.disabled,
  .arrow-right.disabled {
    pointer-events: none;
    user-select: none;
    color: #eaeaea;
  }

  .left-calendar,
  .right-calendar {
    display: inline-table;
    text-align: center;
    padding: 5px;
    box-sizing: border-box;
  }

  .right-calendar {
    margin-left: 20px;
  }

  .title {
    font-weight: bold;
    user-select: none;
    margin-bottom: 10px;
    color: #35495E;
  }

  table {
    table-layout: fixed;
  }

  thead td {
    cursor: initial;
    pointer-events: none;
    user-select: none;
  }

  table td {
    cursor: pointer;
    user-select: none;

    width: 30px;
    height: 30px;
    box-sizing: border-box;
    font-size: 14px;
    border-radius: 3px;
  }

  td.current {
    font-weight: bold;
  }

  td:hover {
    background: #eaeaea;
  }

  td.nil {
    color: #ccc;
    background: #fff;
    pointer-events: none;
    user-select: none;
  }

  td.potential {
    background: #eaeaea;
  }

  td.selected {
    background: #41B883;
    color: #fff;
    font-weight: 600;
  }

  td.ranged {
    background: rgba(79, 190, 140, 0.67);
    color: #fff;
  }
</style>

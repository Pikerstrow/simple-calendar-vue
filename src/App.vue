<template>
  <div id="app">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-2 text-center">
          <img  width="40" src="./assets/logo.png" alt="vue-logo">
        </div>
      </div>
      <div class="row">
        <div class="col-12">
          <h2 class="text-center">Календар</h2>
          <hr>          
        </div>
      </div>      
      <div class="row justify-content-center">
        <div class="col-12">
          <div class="calendar-wrapper">
            <h3 class="text-center">
              <span class="month-switcher" @click="prevMonth()"> &lsaquo;&lsaquo; </span>              
              {{ defaultMonth }}
              <span class="month-switcher" @click="nextMonth()"> &rsaquo;&rsaquo; </span>              
            </h3>
            <hr>
            <table class="table table-calendar">
              <thead>
                <tr>
                  <th class="text-center">Пн</th>
                  <th class="text-center">Вт</th>
                  <th class="text-center">Ср</th>
                  <th class="text-center">Чт</th>
                  <th class="text-center">Пт</th>
                  <th class="text-center">Сб</th>
                  <th class="text-center">Нд</th>
                </tr>
              </thead>
              <tbody v-html="seedDays()"></tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      month: new Date().getMonth(),
      year: new Date().getFullYear(),
      todaysDate: new Date().getDate(),
      todaysMonth: new Date().getMonth(),
      todaysYear: new Date().getFullYear()
    };
  },
  computed: {
    defaultMonth() {
      return this.getStartDateForTitle();
    },
    date() {
      return new Date(this.year, this.month);
    }
  },
  methods: {
    getStartDateForTitle() {
      let options = {
        year: "numeric",
        month: "long",
        timezone: "UTC+2"
      };
      return this.date.toLocaleString("UK-ua", options);
    },
    getDaysQuantityInMonth(year, month) {
      return new Date(year, month, 0).getDate();
    },
    nextMonth() {
      let currentMonth = this.month;

      if (currentMonth == 11) {
        this.month = 0;
        this.year++;
      } else {
        this.month++;
      }
    },
    prevMonth() {
      let currentMonth = this.month;
      let prevMonth = null;

      if (currentMonth == 0) {
        this.month = 11;
        this.year--;
      } else {
        this.month--;
      }
    },
    seedDays() {
      let daysQuantity = this.getDaysQuantityInMonth(this.year, this.month + 1);

      let previousMonthDaysQuantity = this.getDaysQuantityInMonth(
        this.year,
        this.month
      );
      let nextMonthDaysQuantity = this.getDaysQuantityInMonth(
        this.year,
        this.month + 2
      );

      let weeksQuantity = 6;

      let startDayOfMonth = this.getDayForLoop(
        new Date(this.year, this.month, 1).getDay()
      );
      let lastDayOfMonth = this.getDayForLoop(
        new Date(this.year, this.month, daysQuantity).getDay()
      );

      let end = 42; // 6 weeks * 7 days;

      let result = "<tr>";

      for (let i = 1; i <= end; i++) {
        if (i < startDayOfMonth) {
          result +=
            "<td class='text-center day-not-exists'>" +
            (previousMonthDaysQuantity - (startDayOfMonth - i - 1)) +
            "</td>";
        } else if (i - startDayOfMonth >= daysQuantity) {
          if (i % 7 === 0) {
            result +=
              "<td class='text-center day-not-exists'>" +
              (i - daysQuantity - startDayOfMonth + 1) +
              "</td></tr><tr>";
          } else {
            result +=
              "<td class='text-center day-not-exists'>" +
              (i - daysQuantity - startDayOfMonth + 1) +
              "</td>";
          }
        } else {
          let delta = startDayOfMonth - 1; // because days counts from 1;
          let dayOfWeek = new Date(this.year, this.month, i - delta).getDay();

          if (dayOfWeek === 0) {
            if (
              this.month === this.todaysMonth &&
              this.year === this.todaysYear &&
              i - delta === this.todaysDate
            ) {
              result +=
                '<td class="text-center day-exists current-date">' +
                (i - delta) +
                "</td></tr><tr>";
            } else {
              result +=
                '<td class="text-center day-exists">' +
                (i - delta) +
                "</td></tr><tr>";
            }
          } else {
            if (
              this.month === this.todaysMonth &&
              this.year === this.todaysYear &&
              i - delta === this.todaysDate
            ) {
              result +=
                '<td class="text-center day-exists current-date">' +
                (i - delta) +
                "</td>";
            } else {
              result +=
                '<td class="text-center day-exists">' + (i - delta) + "</td>";
            }
          }
        }
      }

      result += "</tr>";

      return result;
    },
    getDayForLoop(num) {
      switch (num) {
        case 0:
          return 7;
          break;
        case 1:
          return 1;
          break;
        case 2:
          return 2;
          break;
        case 3:
          return 3;
          break;
        case 4:
          return 4;
          break;
        case 5:
          return 5;
          break;
        case 6:
          return 6;
          break;
      }
    }
  }
};
</script>

<style>
.table-calendar thead {
  background-color: grey;
  color: white;
  border: 2px solid grey;
}
.table-calendar thead th {
  border: 1px solid white;
}
.table-calendar td {
  height: 50px;
}
.calendar-wrapper {
  width: 100%;
  border: 2px solid darkgreen;
  padding: 20px;
}
.day-not-exists {
  background: rgb(223, 221, 221);
  border: 1px solid green;
  padding: 20px;
  color: grey;
}
.day-exists {
  border: 1px solid green;
}
.current-date {
  background: lightblue;
}
.month-switcher{
  cursor: pointer;
}
</style>

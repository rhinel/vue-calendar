<template>
  <div class="vue-calendar">
    <div class="vue-calendar-top-bar" :class="{
        hide: !monthOpen
      }">
      <span @click="prevMonth()">
        <svg width="16" height="16" viewBox="0 0 16 16" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g class="transform-group"><g transform="scale(0.015625, 0.015625)"><path d="M671.968 912c-12.288 0-24.576-4.672-33.952-14.048L286.048 545.984c-18.752-18.72-18.752-49.12 0-67.872l351.968-352c18.752-18.752 49.12-18.752 67.872 0 18.752 18.72 18.752 49.12 0 67.872l-318.016 318.048 318.016 318.016c18.752 18.752 18.752 49.12 0 67.872C696.544 907.328 684.256 912 671.968 912z" :fill="highlight"></path></g></g></svg>
      </span>
      <span class="yam">
        {{y}}{{text.year}}{{m + 1}}{{text.month}}
      </span>
      <span @click="nextMonth()">
        <svg width="16" height="16" viewBox="0 0 16 16" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g class="transform-group"><g transform="scale(0.015625, 0.015625)"><path d="M761.056 532.128c0.512-0.992 1.344-1.824 1.792-2.848 8.8-18.304 5.92-40.704-9.664-55.424L399.936 139.744c-19.264-18.208-49.632-17.344-67.872 1.888-18.208 19.264-17.376 49.632 1.888 67.872l316.96 299.84-315.712 304.288c-19.072 18.4-19.648 48.768-1.248 67.872 9.408 9.792 21.984 14.688 34.56 14.688 12 0 24-4.48 33.312-13.44l350.048-337.376c0.672-0.672 0.928-1.6 1.6-2.304 0.512-0.48 1.056-0.832 1.568-1.344C757.76 538.88 759.2 535.392 761.056 532.128z" :fill="highlight"></path></g></g></svg>
      </span><span class="totoday">
        <span v-show="!todayIsSelect()" @click="selectToday()" :style="{
          borderColor: highlight
        }">{{text.today}}</span>
      </span>
    </div>
    <div class="vue-calendar-week" @click="trgWeek()">
      <span class="week-day" v-for="(item, index) in weekDay" :style="{
          color: (item == '六' ||  item == '日') ? highlight : ''
        }">
        {{item}}
      </span>
    </div>
    <div class="vue-calendar-sroll">
      <div class="vue-calendar-content" :class="{
          next: isNext,
          prev: isPrev,
          change: changeIng
        }" :style="{
          transform: 'translateX(' + (srollTouch.touchMoveX - srollTouch.touchStartX) + 'px)'
        }" @touchstart="touchstart($event)" @touchmove="touchmove($event)" @touchend="touchend($event)" @touchcancel="touchend($event)">
        <div class="vue-calendar-days" :class="{
            hide: !monthOpen
          }" :style="{
            height: (dates.length / 7) * 40 + 'px'
          }">
          <div class="dates-wrap" :style="{
              top: positionTop + 'px'
            }">
            <span class="day" v-for="(item, index) in prevDates" :style="{
                color: (item.day == 6 ||  item.day == 7) ? highlight : ''
              }">
              <span class="select" :class="{
                  nolm: !item.lm,
                  today: isToday(item.year, item.month, item.date),
                  on: isSelect(item.year, item.month, item.date)
                }" :style="{
                  backgroundColor: isSelect(item.year, item.month, item.date) ? highlight : '',
                  borderColor: isToday(item.year, item.month, item.date) ? highlight : ''
                }">
                {{zero(item.date)}}
              </span>
            </span>
          </div>
        </div>
        <div class="vue-calendar-days" :class="{
            hide: !monthOpen
          }" :style="{
            height: (dates.length / 7) * 40 + 'px'
          }">
          <div class="dates-wrap" :style="{
              top: positionTop + 'px'
            }">
            <span class="day" v-for="(item, index) in dates" :style="{
                color: (item.day == 6 ||  item.day == 7) ? highlight : ''
              }" @click="selectOne(item)">
              <span class="select" :class="{
                  nolm: !item.lm,
                  today: isToday(item.year, item.month, item.date),
                  on: isSelect(item.year, item.month, item.date)
                }" :style="{
                  backgroundColor: isSelect(item.year, item.month, item.date) ? highlight : '',
                  borderColor: isToday(item.year, item.month, item.date) ? highlight : ''
                }">
                {{zero(item.date)}}
              </span>
            </span>
          </div>
        </div>
        <div class="vue-calendar-days" :class="{
            hide: !monthOpen
          }" :style="{
            height: (dates.length / 7) * 40 + 'px'
          }">
          <div class="dates-wrap" :style="{
              top: positionTop + 'px'
            }">
            <span class="day" v-for="(item, index) in nextDates" :style="{
                color: (item.day == 6 ||  item.day == 7) ? highlight : ''
              }">
              <span class="select" :class="{
                  nolm: !item.lm,
                  today: isToday(item.year, item.month, item.date),
                  on: isSelect(item.year, item.month, item.date)
                }" :style="{
                  backgroundColor: isSelect(item.year, item.month, item.date) ? highlight : '',
                  borderColor: isToday(item.year, item.month, item.date) ? highlight : ''
                }">
                {{zero(item.date)}}
              </span>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'vue-calendar',
  props: {
    highlight: {
      type: String,
      default: '#000'
    },
    weekstart: {
      type: Number,
      default: 7
    },
    selectdate: {
      type: [Date, String],
      default () {
        return new Date()
      }
    },
    callback: {
      type: Function,
      default () {
        return () => {}
      }
    }
  },
  data () {
    return {
      el: null,
      text: {
        year: '年',
        month: '月',
        week: ['一', '二', '三', '四', '五', '六', '日'],
        today: '今'
      },
      y: new Date().getFullYear(),
      m: new Date().getMonth(),
      ny: 0,
      nm: 0,
      py: 0,
      pm: 0,
      dates: [],
      nextDates: [],
      prevDates: [],
      selectDate: this.selectdate,
      monthOpen: true,
      positionTop: 0,
      changeIng: false,
      isNext: false,
      isPrev: false,
      srollTouch: {
        touchStartX: 0,
        touchStartY: 0,
        touchMoveX: 0,
        touchMoveY: 0,
        touchEndX: 0,
        touchEndY: 0
      }
    }
  },
  created () {
    this.dates = this.monthDay(this.y, this.m)
    this.getNextMonthDate()
    this.getPrevMonthDate()
    // this.trgWeek()
  },
  mounted () {
    this.el = document.querySelector('.vue-calendar')
  },
  computed: {
    weekDay () {
      return this.text.week.slice(this.weekstart - 1).concat(this.text.week.slice(0, this.weekstart - 1))
    }
  },
  methods: {
    zero (n) {
      return n < 10 ? '0' + n : n
    },
    monthDay (y, m) {
      let firstDayOfMonth = new Date(y, m, 1).getDay()                // 当月第一天星期几
      let lastDateOfMonth = new Date(y, m + 1, 0).getDate()           // 当月最后一天
      let lastDayOfLastMonth = new Date(y, m, 0).getDate()            // 上一月的最后一天
      let dates = []                                                  // 所有渲染日历
      let weekstart = this.weekstart == 7 ? 0 : this.weekstart        // 方便进行日期计算，默认星期从0开始
      let startDay = (() => {                                         // 周初有几天是上个月的
        if (firstDayOfMonth == weekstart) {
          return 0
        } else if (firstDayOfMonth > weekstart) {
          return firstDayOfMonth - weekstart
        } else {
          return 7 - weekstart + firstDayOfMonth
        }
      })()
      let endDay = 7 - (startDay + lastDateOfMonth) % 7               // 结束还有几天是下个月的
      for (let i = 1; i <= startDay; i++) {
        dates.push({
          date: lastDayOfLastMonth - startDay + i,
          day: (weekstart + i - 1) || 7,
          month: m - 1 >= 0 ? m - 1 : 12,
          year: m - 1 >= 0 ? y : y - 1
        })
      }
      for (let j = 1; j <= lastDateOfMonth; j++) {
        dates.push({
          date: j,
          day: (j % 7 + firstDayOfMonth - 1) || 7,
          month: m,
          year: y,
          lm: true
        })
      }
      for (let k = 1; k <= endDay; k++) {
        dates.push({
          date: k,
          day: (lastDateOfMonth + startDay + weekstart + k - 1) % 7 || 7,
          month: m + 1 <= 11 ? m + 1 : 0,
          year: m + 1 <= 11 ? y : y + 1,
          select: false
        })
      }
      return dates
    },
    isToday (y, m, d) {
      let date = new Date()
      return y == date.getFullYear() && m == date.getMonth() && d == date.getDate()
    },
    isSelect (y, m, d) {
      let selectDate = new Date(this.selectDate)
      return y == selectDate.getFullYear() && m == selectDate.getMonth() && d == selectDate.getDate()
    },
    todayIsSelect () {
      return new Date(this.selectDate).toLocaleDateString() == new Date().toLocaleDateString()
    },
    selectOne (i) {
      this.selectDate = new Date(i.year, i.month, i.date)
      this.callback(i)
    },
    async selectToday () {
      let today = new Date()
      let selectDate = new Date(this.selectDate)
      let select = null
      if (today.getFullYear() > this.y ||
        (today.getFullYear() == this.y &&
          today.getMonth() > this.m
        )) {
        this.getNextMonthDate(today.getFullYear(), today.getMonth())
        this.nextMonth()
        await this.takeBack(300)
      } else if (today.getFullYear() < this.y ||
        (today.getFullYear() == this.y &&
          today.getMonth() < this.m
        )) {
        this.getPrevMonthDate(today.getFullYear(), today.getMonth())
        this.prevMonth()
        await this.takeBack(300)
      }
      this.dates.forEach((i, x) => {
        this.isToday(i.year, i.month, i.date) && (select = i)
      })
      this.selectOne(select)
    },
    trgWeek () {
      this.monthOpen = !this.monthOpen
      if (this.monthOpen) {
        this.positionTop = 0
      } else {
        let index = -1
        this.dates.forEach((i, x) => {
          this.isSelect(i.year, i.month, i.date) && (index = x)
        })
        this.positionTop = - ((Math.ceil((index + 1) / 7) || 1) - 1) * 40
      }
    },
    getNextMonth (y, m) {
      return {
        m: m + 1 <= 11 ? m + 1 : 0,
        y: m + 1 <= 11 ? y : y + 1
      }
    },
    getPrevMonth (y, m) {
      return {
        m: m - 1 >= 0 ? m - 1 : 11,
        y: m - 1 >= 0 ? y : y - 1
      }
    },
    getNextMonthDate (y, m) {
      let next = this.getNextMonth(this.y, this.m)
      this.ny = y || next.y
      this.nm = m || next.m
      this.nextDates = this.monthDay(this.ny, this.nm)
    },
    getPrevMonthDate (y, m) {
      let prev = this.getPrevMonth(this.y, this.m)
      this.py = y || prev.y
      this.pm = m || prev.m
      this.prevDates = this.monthDay(this.py, this.pm)
    },
    async nextMonth (type) {
      if (this.changeIng && !type) return
      this.changeIng = true
      this.isNext = true
      await this.takeBack(300)
      this.dates = this.nextDates
      this.y = this.ny
      this.m = this.nm
      this.srollDone()
    },
    async prevMonth (type) {
      if (this.changeIng && !type) return
      this.changeIng = true
      this.isPrev = true
      await this.takeBack(300)
      this.dates = this.prevDates
      this.y = this.py
      this.m = this.pm
      this.srollDone()
    },
    takeBack (time) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve()
        }, time)
      })
    },
    srollDone () {
      this.changeIng = false
      this.isNext = this.isPrev = false
      this.getNextMonthDate()
      this.getPrevMonthDate()
    },
    touchstart (e) {
      if (!this.monthOpen) return
      this.srollTouch.touchStartX = this.srollTouch.touchMoveX = e.targetTouches[0].pageX
      this.srollTouch.touchStartY = this.srollTouch.touchMoveY = e.targetTouches[0].pageY
    },
    touchmove (e) {
      if (!this.monthOpen) return
      this.srollTouch.touchMoveX = e.targetTouches[0].pageX
      this.srollTouch.touchMoveY = e.targetTouches[0].pageY
    },
    async touchend (e) {
      if (!this.monthOpen) return
      this.changeIng = true
      let elWidth = this.el.offsetWidth / 7
      let range = this.srollTouch.touchMoveX - this.srollTouch.touchStartX
      if (- range > elWidth) {
        this.nextMonth(true)
        this.srollTouch.touchStartX = this.srollTouch.touchMoveX = 0
      } else if (range > elWidth) {
        this.prevMonth(true)
        this.srollTouch.touchStartX = this.srollTouch.touchMoveX = 0
      } else {
        this.srollTouch.touchStartX = this.srollTouch.touchMoveX = 0
        await this.takeBack(300)
        this.changeIng = false
      }
      this.srollTouch.touchEndX = e.changedTouches[0].pageX
      this.srollTouch.touchEndY = e.changedTouches[0].pageY
    }
  }
}
</script>

<style lang="scss" scoped>
.vue-calendar {
  user-select: none;
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: Helvetica Neue,Arial,PingFang SC,Hiragino Sans GB,STHeiti,Microsoft YaHei,WenQuanYi Micro Hei,sans-serif;
  }
  &-top-bar {
    text-align: center;
    line-height: 0;
    height: 36px;
    overflow: hidden;
    transition: height 0.3s;
    &.hide {
      height: 0!important;
    }
    & > span {
      display: inline-block;
      vertical-align: top;
      padding: 10px 0;
    }
    svg {
      vertical-align: top;
      display: inline-block;
      cursor: pointer;
    }
    .yam {
      line-height: 100%;
      margin-top: -0.1%;
      margin-right: 10px;
      margin-left: 10px;
    }
    .totoday {
      position: relative;
      height: 100%;
      padding: 0;
      & > span {
        position: absolute;
        display: block;
        top: calc(18px - 15px);
        right: -3em;
        line-height: 29px;
        height: 30px;
        width: 30px;
        border: 1px solid transparent;
        border-radius: 50%;
      }
    }
  }
  &-week {
    display: flex;
    padding: 10px 0;
    cursor: pointer;
    .week-day {
      display: inline-block;
      line-height: 100%;
      margin-top: -0.1%;
      text-align: center;
      flex: 1;
    }
  }
  &-sroll {
    width: 100%;
    overflow: hidden;
  }
  &-content {
    width: 300%;
    display: flex;
    position: relative;
    left: -100%;
    transform: translateX(0);
    &.change {
      transition: transform 0.3s;
    }
    &.next {
      transform: translateX(-33.3333333%)!important;
    }
    &.prev {
      transform: translateX(33.3333333%)!important;
    }
  }
  &-days {
    width: 100%;
    overflow: hidden;
    transition: height 0.3s;
    &.hide {
      height: 40px!important;
    }
    .dates-wrap {
      transition: top 0.3s;
      position: relative;
      display: flex;
      flex-wrap: wrap;
    }
    .day {
      padding: 5px 0;
      text-align: center;
      width: calc( 100% / 7 );
      cursor: pointer;
      line-height: 1;
      height: 40px;
    }
    .nolm {
      color: #999;
    }
    .select {
      vertical-align: top;
      display: inline-block;
      height: 30px;
      width: 30px;
      border-radius: 50%;
      line-height: 29px;
      border: 1px solid transparent;
    }
    .select.on {
      color: #fff;
    }
  }
}
</style>

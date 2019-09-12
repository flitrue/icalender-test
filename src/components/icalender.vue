<template>
  <div class="icalender">
    <div class="icalender-header">
      <div class="arrow" @click="forward">
        <svg fill="transparent" viewBox="0 0 30 30">
          <polyline points="20,5 10,15 20,25" stroke="#dcdcdc" stroke-width="4"></polyline>
        </svg>
      </div>
      <div class="title">
        {{ year }}年{{ month_list[month] }}月
      </div>
      <div class="arrow" @click="back">
        <svg fill="transparent" viewBox="0 0 30 30">
          <polyline points="10,5 20,15 10,25" stroke="#dcdcdc" stroke-width="4"></polyline>
        </svg>
      </div>
    </div>

    <div class="icalender-body">
      <div class="week-head">
        <div class="week-item" v-for="item in week_list" :key="item">{{ item }}</div>
      </div>
      <table>
        <tr v-for="(item,index) in list" :key="index">
          <td v-for="(it,idx) in item" :key="idx" :class="it.active?'active-bg':''">
            <div class="point">
              <div :class="{'active-point':now==it.full_day, 'now-point': now==it.full_day}"></div>
              <span class="date-label" :active="now==it.full_day" :title="it.full_day">{{ it.day }}</span>
            </div>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "icalender",
  data() {
    return {
      now: "2019-09-05",
      year: "",
      month: "",
      month_list: ["01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12"],
      week_list: ["日", "一", "二", "三", "四", "五", "六"],
      list: [],
      counts: []
    }
  },
  methods: {
    forward() {
      if(this.month == 0){
        this.year = this.year == 1970 ? 1970 : this.year - 1;
        this.month = 11;
      } else {
        this.month -= 1;
      }
      this.getDayList(this.year, this.month);
    },
    back() {
      if(this.month == 11){
        this.year += 1;
        this.month = 0;
      } else {
        this.month += 1;
      }
      this.getDayList(this.year, this.month);
    },
    getCurrentMonth() {
      let date = new Date();
      return date.getMonth();
    },
    getCurrentYear() {
      let date = new Date();
      return date.getFullYear();
    },
    /** 
     * @return 当前日期某天
    */
    getCurrentDate() {
      let date = new Date();
      return date.getDate();
    },
    /** 
     * @return 当前日期星期
    */
    getCurrentDay() {
      let date = new Date();
      return date.getDay();
    },
    getDayList(year, month, range =[]) {
      if(arguments.length < 2) throw new Error("getDayList必须传入两个参数year和month");
      let counts = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]; // 每年月份天数
      let date = new Date();
      this.list = [];
      if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) {
        counts[1] = 29;
      }
      // 根据当前月1号是星期几 判断需要绘制几列
      let this_week = new Date(year, month, 1).getDay(); // 当前月1号是星期几
      let row = this_week >= 5 ? 6 : 5;// 绘制几列 ? 6 ：5
      let i_list = []
      for(let i=0;i<this_week;i++){
        i_list.push("")
      }
      for(let i = 0;i < counts[month];i++) {
        i_list.push(i+1);
      }
      let arr = []
      for(let i = 0;i < i_list.length;i++) {
        if (i > 0 && i % 7 == 0) {
          this.list.push(arr)
          arr = []
        }
        let obj = {
          day: i_list[i]
        }
        
        if (i_list[i] != '') {
          let day = i_list[i] > 10 ? i_list[i] : "0" + i_list[i];
          obj.full_day = year + "-" + this.month_list[month] + "-" + day;
        }
        if(range.length > 0){
          if (obj.day >= range[0] && obj.day <= range[1]) {
            obj.active = true;
          }
        }
        arr.push(obj);
      }
      this.list.push(arr);
    }
  },
  created() {
    let date = new Date();
    this.month = date.getMonth();
    this.year = date.getFullYear();
    let day = date.getDate();
    day < 10 && (day = "0" + day)
    //this.now =  this.year + "-" + this.month_list[this.month] + "-" + day;
    this.getDayList(this.year, this.month, [2, 5]);
  }
}
</script>

<style scoped>
svg{
  width: 16px;
  height: 16px;
  vertical-align: middle;
}

.arrow{
  display: inline-block;
  background-color: rgba(31,45,61,.11);
  width: 30px;
  height: 30px;
  line-height: 28px;
  text-align: center;
  border-radius: 50%;
  transition: .2s;
  cursor: pointer;
}

.arrow:hover{
  background-color: rgba(31,45,61,.5);
}

.title{
  display: inline-block;
  margin: auto 0;
  text-align: center;
  flex: 1;
  font-weight: 500;
}

.icalender{
  width: 300px;
  user-select: none;
}

.icalender-header{
  display: flex;
}

.icalender-body{
 
}
.icalender-body table{
  width: 100%;
  border-collapse: collapse;
}

.icalender-body td{
  text-align: center;
}

.week-head{
  display: flex;
  justify-content: space-around;
  font-weight: 500;
}

.point{
  position: relative;
  display: inline-block;
  width: 30px;
  height: 30px;
  line-height: 30px;
  font-weight: 500;
}
.active-point{
  position: absolute;
  border-radius: 50%;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #D0494D;
  z-index: 2;
}

.now-point{
  background-color: rgb(12, 69, 155);
}

.date-label{
  position: relative;
  z-index: 4;
}
.date-label[active]{
  color: #dcdcdc;
}

.active-bg{
  background-color: #F1868D;
  color: #dcdcdc;
}
</style>

<template>
  <div>
    <el-card style="margin: 10px 0">
      <div slot="header" class="clearfix">
        <el-tabs v-model="activeName" class="tab">
          <el-tab-pane label="销售额" name="sale" />
          <el-tab-pane label="访问量" name="visit" />
        </el-tabs>
        <div class="right">
          <span @click="setDay">今日</span>
          <span @click="setWeek">本周</span>
          <span @click="setMonth">本月</span>
          <span @click="setYear">本年</span>
          <el-date-picker
            v-model="date"
            class="date"
            type="daterange"
            range-separator="-"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            size="mini"
            value-format="yyyy-MM-dd"
          />
        </div>
      </div>
      <div>
        <el-row :gutter="10">
          <el-col :span="18">
            <div ref="charts" class="charts" />
          </el-col>
          <el-col :span="6">
            <!-- <div class="right"> -->
            <h3>门店{{ title }}趋势</h3>
            <ul>
              <li>
                <span class="rindex">0</span>
                <span> 肯德基 </span>
                <span class="rvalue"> 14559 </span>
              </li>
              <li>
                <span class="rindex">1</span>
                <span> 肯德基 </span>
                <span class="rvalue"> 14559 </span>
              </li>
              <li>
                <span class="rindex">2</span>
                <span> 肯德基 </span>
                <span class="rvalue"> 14559 </span>
              </li>
              <li>
                <span class="">3</span>
                <span> 肯德基 </span>
                <span class="rvalue"> 14559 </span>
              </li>
              <li>
                <span class="">4</span>
                <span> 肯德基 </span>
                <span class="rvalue"> 14559 </span>
              </li>
              <li>
                <span class="">5</span>
                <span> 肯德基 </span>
                <span class="rvalue"> 14559 </span>
              </li>
            </ul>
            <!-- </div> -->
          </el-col>
        </el-row>
      </div>
    </el-card>
  </div>
</template>

<script>
import * as echarts from 'echarts'
import dayjs from 'dayjs'
export default {
  data() {
    return {
      activeName: 'sale',
      mycharts: null,
      date: []
    }
  },
  computed: {
    title() {
      return this.activeName == 'sale' ? '销售额' : '访问量'
    }
  },
  watch: {
    title() {
      this.mycharts.setOption({
        title: {
          text: this.title
        }
      })
    }
  },
  mounted() {
    this.mycharts = echarts.init(this.$refs.charts)
    this.mycharts.setOption({
      title: {
        text: '销售额'
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'shadow'
        }
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      },
      xAxis: [
        {
          type: 'category',
          data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
          axisTick: {
            alignWithLabel: true
          }
        }
      ],
      yAxis: [
        {
          type: 'value'
        }
      ],
      series: [
        {
          name: 'Direct',
          type: 'bar',
          barWidth: '60%',
          data: [10, 52, 200, 334, 390, 330, 220, 12, 34, 56, 76, 43, 67, 89]
        }
      ]
    })
  },
  methods: {
    setDay() {
      const day = dayjs().format('YYYY-MM-DD')
      this.date = [day, day]
    },
    setWeek() {
      const start = dayjs().day(1).format('YYYY-MM-DD')
      const end = dayjs().day(7).format('YYYY-MM-DD')
      this.date = [start, end]
    },
    setMonth() {
      const start = dayjs().startOf('month').format('YYYY-MM-DD')
      const end = dayjs().endOf('month').format('YYYY-MM-DD')
      this.date = [start, end]
    },
    setYear() {
      const start = dayjs().startOf('year').format('YYYY-MM-DD')
      const end = dayjs().endOf('year').format('YYYY-MM-DD')
      this.date = [start, end]
    }
  }

}
</script>

<style lang="scss" scoped>
.clearfix {
  position: relative;
  display: flex;
  justify-content: space-between;
}

.tab {
  width: 100%;
}
.right {
  position: absolute;
  right: 0;
}

.right span {
  margin: 0 20px;
}

.date {
  width: 200px;
  margin: 0 20px;
}

.charts {
  width: 100%;
  height: 300px;
}

ul {
  list-style: none;
  width: 100%;
  height: 300px;
  padding: 0;
}

ul li {
  height: 8%;
  margin: 10px 0;
}

.rindex {
  float: left;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  text-align: center;
  background-color: #000;
  color: white;
  margin-right: 10px;
}

.rvalue {
  float: right;
}
</style>

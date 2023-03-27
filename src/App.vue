<!-- <template>
  <div ref="chart" style="height: 400px;"></div>
</template>
<script>
import * as echarts from "echarts"
import axios from "axios"

export default{
 name: 'App',
  data() {
  return {
    chartData: {
      title: {
        text: 'Pie Chart'
      },
      tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
      },
      legend: {
        orient: 'vertical',
        left: 10,
        data: []
      },
      series: [
        {
          name: 'Data',
          type: 'pie',
          radius: '100%',
          center: ['50%', '40%'],
          data: [],
          itemStyle: {
            emphasis: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ]
    }
  }
},
mounted() {
  axios.get('http://127.0.0.1:8000/api/v1/stats')
    .then(response => {
      const stats = response.data.data.users

      // Update the legend data with the stats names
      this.chartData.legend.data = stats.map(stat => stat.day)

      // Update the series data with the stats values
      this.chartData.series[0].data = stats.map(stat => ({
        name: stat.user_id,
        value: (stat.activities / stat.total_activities) * 100
      }))

      // Create the chart with the updated configuration and data
      const chart = echarts.init(this.$refs.chart)
      chart.setOption(this.chartData)
    })
    .catch(error => {
      console.error(error)
    })
},
watch: {
  '$store.state.stats': function (stats) {
    // Update the legend data with the stats names
    this.chartData.legend.data = stats.map(stat => stat.day)

    // Update the series data with the stats values
    this.chartData.series[0].data = stats.map(stat => ({
      name: stat.user_id,
      value: (stat.activities / stat.total_activities) * 100
    }))

    // Update the chart with the updated data
    const chart = echarts.init(this.$refs.chart)
    chart.setOption(this.chartData)
  }
}

}
</script>
 -->
 <template>
  <div ref="chart" style="height: 400px;"></div>
</template>

<script>
import * as echarts from "echarts"
import axios from "axios"

export default{
  name: 'App',
  data() {
    return {
      chartData: {
        title: {
          text: 'Area Chart'
        },
        tooltip: {
          trigger: 'axis',
          formatter: '{a} <br/>{b}: {c}'
        },
        legend: {
          data: []
        },
        xAxis: {
          type: 'category',
          data: []
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            name: 'Data',
            type: 'line',
            smooth: true,
            areaStyle: {},
            data: []
          }
        ]
      }
    }
  },
  mounted() {
    axios.get('http://127.0.0.1:8000/api/v1/stats')
      .then(response => {
        const stats = response.data.data.users

        // Update the legend data with the stats names
        this.chartData.legend.data = stats.map(stat => stat.user_id)

        // Update the x-axis data with the stats day
        this.chartData.xAxis.data = stats.map(stat => stat.day)

        // Update the series data with the stats values
        this.chartData.series[0].data = stats.map(stat => stat.activities)

        // Create the chart with the updated configuration and data
        const chart = echarts.init(this.$refs.chart)
        chart.setOption(this.chartData)
      })
      .catch(error => {
        console.error(error)
      })
  },
  watch: {
    '$store.state.stats': function (stats) {
      // Update the legend data with the stats names
      this.chartData.legend.data = stats.map(stat => stat.user_id)

      // Update the x-axis data with the stats day
      this.chartData.xAxis.data = stats.map(stat => stat.day)

      // Update the series data with the stats values
      this.chartData.series[0].data = stats.map(stat => stat.activities)

      // Update the chart with the updated data
      const chart = echarts.init(this.$refs.chart)
      chart.setOption(this.chartData)
    }
  }
}
</script>

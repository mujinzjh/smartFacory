<template>
	<div :class="className" :style="{ height: height, width: width }" />
</template>

<script>
import * as echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'

const animationDuration = 6000

export default {
	mixins: [resize],
	props: {
		className: {
			type: String,
			default: 'chart'
		},
		width: {
			type: String,
			default: '100%'
		},
		height: {
			type: String,
			default: '300px'
		},
		chartData: {
			type: Object,
			required: true
		}
	},
	data() {
		return {
			chart: null
		}
	},
	watch: {
		chartData: {
			deep: true,
			handler(val) {
				this.$nextTick(() => {
					this.initChart()
				})
			}
		}
	},
	mounted() {
		this.$nextTick(() => {
			this.initChart()
		})
	},
	beforeDestroy() {
		if (!this.chart) {
			return
		}
		this.chart.dispose()
		this.chart = null
	},
	methods: {
		initChart() {
			this.chart = echarts.init(this.$el, 'macarons')

			this.chart.setOption({
				tooltip: {
					trigger: 'axis',
					axisPointer: { // 坐标轴指示器，坐标轴触发有效
						type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
					}
				},
				title: {
					text: '不良数量',
					left: 'center',
					textStyle: {
					  color: '#fff',
						fontSize: 16
					}
				},
				grid: {
					top: "10%",
					left: '2%',
					right: '2%',
					bottom: '3%',
					containLabel: true
				},
				xAxis: [{
					type: 'category',
					data: this.chartData.xData,
					axisTick: {
						alignWithLabel: true
					}
				}],
				yAxis: [{
					type: 'value',
					axisTick: {
						show: false
					}
				}],
				series: [{
					name: '不良数量',
					type: 'bar',
					barWidth: 30,
					data: this.chartData.yData,
					animationDuration
				}]
			})
		}
	}
}
</script>

<template>
	<view class="content">
		<u-button style="margin-bottom: 30rpx;" type="primary" @click="show = true" text="提醒时间"></u-button>
		<u-picker :show="show" ref="uPicker" :columns="columns" @cancel="show = false" @confirm="confirm" @change="changeHandler"></u-picker>
		<u-button style="margin-bottom: 30rpx;" type="primary" @click="handleOpen" text="开启"></u-button>
		<u-count-down v-show="isCountDown" @finish="handleFinish" :time="minutes *60 * 1000" format="mm:ss"></u-count-down>
		<!-- 30 * 60 * 60 * 1000 -->
		<!-- 26 * 60 *1000 mm:ss -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isCountDown: false,
				minutes: 26,
				show: false,
				columns: [
					['1','26', '27','28','29','30','31','32','33','34','35','36','37','38','39','40','41','42','43','44','45','46','47','48','49','50'],
					['00']
				],
				columnData: [
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00'],
					['00']									
				]
}
		},
		onLoad() {

		},
		methods: {
				//倒计时结束
				handleFinish() {
					const innerAudioContext = uni.createInnerAudioContext();
					innerAudioContext.autoplay = true;
					innerAudioContext.src = 'http://www.axuan.pub/audio/xuanzhuren.mp3';
					innerAudioContext.onPlay(() => {
					  console.log('开始播放');
					});
					innerAudioContext.onError((res) => {
					  console.log(res.errMsg);
					  console.log(res.errCode);
					});
				},
				
				  // 开启
				  handleOpen() {
					  // 倒计时显示
					  this.isCountDown = true
					  
				  },
			
		            changeHandler(e) {
		                const {
		                    columnIndex,
		                    value,
		                    values, // values为当前变化列的数组内容
		                    index,
							// 微信小程序无法将picker实例传出来，只能通过ref操作
		                    picker = this.$refs.uPicker
		                } = e
		                // 当第一列值发生变化时，变化第二列(后一列)对应的选项
		                if (columnIndex === 0) {
		                    // picker为选择器this实例，变化第二列对应的选项
		                    picker.setColumnValues(1, this.columnData[index])
		                }
		            },
					// 回调参数为包含columnIndex、value、values
					confirm(e) {
		                console.log('confirm', e)
						this.minutes = e.value[0]
		                this.show = false
					}
				}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>

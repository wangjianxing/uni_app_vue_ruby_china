<template>
	<view class="content">
		<rich-text class="richText" :nodes="content"></rich-text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '',
				content: '',
			}
		},
		onLoad:function(e){
			console.log(e);
			uni.request({
				url: 'https://ruby-china.org/api/v3/topics/' + e.item_id + '.json',
				method: 'GET',
				data: {},
				success: res => {
							console.log(res);
							this.title = res.data.topic.title;
							this.content = res.data.topic.body_html;
						},
				fail: () => {},
				complete: () => {
					uni.setNavigationBarTitle({
						title: this.title,
					})
				}
			});
		},
		methods: {
			
		}
	}
</script>

<style>

</style>

<template>
	<view>
			<switch @change="typeChanged" />
		<uni-list>
			<uni-list-item v-for="(item, index) in topics" :key="item.id" :title="item.title" :thumb="item.user.avatar_url"
			:note="item.user.name" :rightText="(item.excellent ? '💎\t' : '') + item.node_name" :show-badge="true" :badge-text="item.replies_count" @click="opentopic(index)"></uni-list-item>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				topics: [],
				topicType: 'last_actived',
			}
		},
		onLoad:function(){
			this.fetchTopics();
		},
		methods: {
          opentopic(e) {
			var item_id = this.topics[e].id
			uni.navigateTo({
				url: '../topic/topic?item_id=' + item_id,
			});  
		  },
		  typeChanged(e) {
			if (e.target.value == true) {
			  this.topicType = 'excellent';
			  uni.setNavigationBarTitle({
			  	title: "💎 ruby-china 精华帖"
			  });
			}
			else {
				this.topicType = 'last_actived';
				uni.setNavigationBarTitle({
					title: "ruby-china 最新帖"
				});
			}
			  
			this.fetchTopics();
		  },
		  fetchTopics() {
			  uni.request({
			  	url: 'https://ruby-china.org/api/v3/topics.json',
			  	method: 'GET',
			  	data: { type: this.topicType, limit: 50 },
			  	success: res => {
			  				console.log(res);
			  				this.topics = res.data.topics
			  			},
			  	fail: () => {},
			  	complete: () => {}
			  });
		  }
		}
	}
</script>

<style>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
	}
</style>

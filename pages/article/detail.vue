<template>
	<view class="container">
		<view class="tui-news-title">{{detail.title}}</view>
		<view class="tui-sub-info">
			<view class="tui-sub-left">
				<text class="tui-author">{{detail.cate}}</text>
				<text>{{detail.create_time}}</text>
			</view>
			<view class="tui-sub-right">阅读 2453</view>
		</view>
		<view class="tui-news-content">
			<uParse :html="detail.content" ref="article" :tag-style="tagStyle"></uParse>
		</view>

		<!--<view class="tui-news-source">消息参考来源：体坛大精汇</view>-->

		<!--<view class="tui-operate-box">
			<<tui-tag padding="26rpx 56rpx" :type="isFabulous ? 'primary' : 'gray'" shape="circle" :plain="true" @click="btnFabulous">
				<tui-icon :name="iconName(isFabulous)" :size="20" :color="iconColor"></tui-icon>
				<text class="tui-black" :class="[isFabulous ? 'tui-primary' : '']">{{ fabulous }}赞</text>
			</tui-tag>
			<button open-type="share" class="tui-share-btn" @tap="btnShare">
				<tui-tag padding="26rpx 60rpx" type="gray" shape="circle" :plain="true">
					<tui-icon name="partake" :size="20" color="#333"></tui-icon>
					<text class="tui-black">分享</text>
				</tui-tag>
			</button>
		</view>-->

		<!--<view class="tui-cmt-title">精彩评论（20）</view>
		<view class="tui-cmtbox">
			<view class="tui-cmt-cell" v-for="(item, index) in cmtList" :key="index">
				<image :src="'/static/images/news/' + item.avatar" class="tui-avatar"></image>
				<view class="tui-cmt-detail">
					<view class="tui-header-box">
						<view class="tui-cmt-nickname">{{ item.nickname }}</view>
						<view class="tui-fabulous" :class="[item.isFabulous ? 'tui-primary' : '']" :id="index" @tap="cmtFabulous">
							<text>{{ item.fabulous == 0 ? '赞' : item.fabulous }}</text>
							<tui-icon :name="iconName(item.isFabulous)" :size="15" :color="itemIconColor(item.isFabulous)"></tui-icon>
						</view>
					</view>
					<view class="tui-cmt-content">{{ item.content }}</view>
					<view class="tui-reply-box" v-if="item.replayNum > 0">
						<tui-list-cell
							backgroundColor="#f2f2f2"
							:size="28"
							v-for="(items, index2) in item.reply"
							:key="index2"
							:unlined="item.replayNum < 2 && item.reply.length - 1 == index"
							@tap="cmtReply"
						>
							<view class="tui-flex-1 tui-reply-nickname">{{ items.nickname }}</view>
							<view class="tui-flex-1">{{ items.content }}</view>
						</tui-list-cell>
						<tui-list-cell padding="20rpx 30rpx" backgroundColor="#f2f2f2" :size="28" :unlined="true" v-if="item.replayNum > 2" @tap="cmtReply">
							<view class="tui-flex-1  tui-cell-last">
								<text>共{{ item.replayNum }}条回复</text>
								<tui-icon name="arrowright" :size="22" color="#5677fc"></tui-icon>
							</view>
						</tui-list-cell>
					</view>
					<view class="tui-footer">
						{{ item.time }}
						<view class="tui-primary tui-ml" hover-class="opcity" :hover-start-time="150" @tap="cmtReply">回复</view>
					</view>
				</view>
			</view>
		</view>

		<view class="tui-operation">
			<view class="tui-operation-left tui-col-7"><view class="tui-btn-comment" @tap="btnCmt">发表你的评论...</view></view>
			<view class="tui-operation-right tui-right-flex tui-col-5">
				<view class="tui-operation-item" hover-class="tui-opcity" :hover-stay-time="150" @tap="cmtAll">
					<tui-icon name="message" :size="30" color="#444"></tui-icon>
					<tui-badge absolute type="white_primary" :scaleRatio="0.7">501</tui-badge>
				</view>
				<view class="tui-operation-item" @tap="collection">
					<tui-icon :name="isCollection ? 'star-fill' : 'star'" :size="29" :color="isCollection ? '#5677fc' : '#444'"></tui-icon>
				</view>
				<view class="tui-operation-item" hover-class="tui-opcity" :hover-stay-time="150" @tap="btnShare">
					<button open-type="share" class="tui-share-btn"><tui-icon name="share" :size="31" color="#444"></tui-icon></button>
				</view>
			</view>
		</view>-->

		<!--加载loadding-->
		<tui-loadmore v-if="loadding" :index="3" type="primary"></tui-loadmore>
		<tui-nomore v-if="!pullUpOn" backgroundColor="#fff" text="没有更多评论"></tui-nomore>
		<!--加载loadding-->
		<view class="tui-safearea-bottom"></view>
	</view>
</template>

<script>
import uParse from '@/components/jyf-parser/jyf-parser';
export default {
	components: {
		uParse
	},
	data() {
		return {
			detail:{},
			fabulous: 123,
			isFabulous: false,
			isCollection: false,
			cmtList: [{
				avatar: "list_2.jpg",
				nickname: "米兰的卡先生",
				fabulous: 123,
				isFabulous: false,
				content: "我一直没懂赛前问一个主教练如何评价对手的主教练， 记者究竟是想得到什么答案？",
				reply: [{
					nickname: "Mesaldo",
					content: "汉军威武!卓尔不凡!火炉德比，热力四射！场上争胜，场下朋友☺ ☻"
				}, {
					nickname: "月牙",
					content: "新政实行后，大小摩托轮流冲，不用经常上迪力了，杨帅该拿下就拿下☺"
				}],
				replayNum: 44,
				time: "昨天 22:12"
			}, {
				avatar: "avatar_1.jpg",
				nickname: "月牙",
				fabulous: 2,
				content: "力帆有杨帅，迪力木来提，尹聪耀，完全可以应付。尤其是杨帅坐稳主力后卫。",
				reply: [{
					nickname: "thorui",
					content: "汉军威武!卓尔不凡!火炉德比，热力四射！场上争胜，场下朋友"
				}, {
					nickname: "Mr卡卜斯",
					content: "说实话，武汉重庆还真的是一家，但是比赛还是要分出个胜负来的，我卓尔球迷肯定是要为我武汉加油了，按照目前两队现在这个状态来看武汉重庆应该是五五开，所以我想说武汉加油！卓尔加油！"
				}],
				replayNum: 2,
				time: "昨天 21:09"
			}, {
				avatar: "avatar_2.jpg",
				nickname: "thorui",
				fabulous: 0,
				content: "小克鲁伊夫带的球队",
				reply: [],
				replayNum: 0,
				time: "昨天 17:30"
			}],
			pageIndex: 1,
			loadding: false,
			pullUpOn: true
		}
	},
	onLoad: function(e) {
		let _this = this;
		_this.$request.get('article.detail', {
			id: e.id
		}).then(res => {
			//console.log(res);
			if (res.errno == 0) {
				_this.detail = res.data;
			}
		});
	},
	computed: {
		iconColor() {
			return this.isFabulous ? '#5677fc' : '#333'
		},
		itemIconColor() {
			return function(isFabulous) {
				return isFabulous ? '#5677fc' : '#9a9a9a'
			}
		},
		iconName() {
			return function(isFabulous) {
				return isFabulous ? 'agree-fill' : 'agree'
			}
		}
	},
	methods: {
		btnFabulous: function() {
			this.fabulous = this.isFabulous ? 123 : 124;
			this.isFabulous = !this.isFabulous
		},
		cmtFabulous: function(e) {
			let index = e.currentTarget.id;
			let fabulousObj = this.cmtList[index];
			let isFabulous = this.cmtList[index].isFabulous;
			let fabulous = this.cmtList[index].fabulous;
			let fabulousNum = isFabulous ? fabulous - 1 : fabulous + 1;
			this.$set(fabulousObj, "fabulous", fabulousNum);
			this.$set(fabulousObj, "isFabulous", !isFabulous);
		},
		collection: function() {
			this.isCollection = !this.isCollection
			if (this.isCollection) {
				this.tui.toast("收藏成功！");
			}
		},
		btnCmt: function() {
			uni.navigateTo({
				url: '../comment/comment'
			})
		},
		cmtAll: function() {
			uni.navigateTo({
				url: '../commentList/commentList'
			})
		},
		cmtReply: function() {
			uni.navigateTo({
				url: '../commentReply/commentReply'
			})
		},
		btnShare(){
			// #ifdef H5
			location.href = 'https://www.thorui.cn/';
			// #endif

			//#ifdef APP-PLUS
			plus.share.sendWithSystem(
				{
					content: 'ThorUI组件库',
					href: 'https://www.thorui.cn/'
				},
				function() {
					console.log('分享成功');
				},
				function(e) {
					console.log('分享失败：' + JSON.stringify(e));
				}
			);
			//#endif
		}
	},
	// 页面上拉触底事件的处理函数
	onReachBottom: function() {
		if (!this.pullUpOn) return;
		this.loadding = true
		if (this.pageIndex == 3) {
			this.loadding = false;
			this.pullUpOn = false
		} else {
			let arr = JSON.parse(JSON.stringify(this.cmtList));
			this.cmtList = this.cmtList.concat(arr);
			this.pageIndex = this.pageIndex + 1;
			this.loadding = false
		}
	}
}
</script>

<style>
page {
	background: #fff;
	color: #333;
}

.container {
	padding: 40rpx 30rpx 110rpx 30rpx;
	box-sizing: border-box;
}

.tui-news-title {
	font-size: 48rpx;
	font-weight: 500;
	text-align: justify;
}

.tui-sub-info {
	padding-top: 30rpx;
	display: flex;
	align-items: center;
	justify-content: space-between;
	font-size: 28rpx;
	color: #999;
}

.tui-author {
	color: #5677fc;
	padding-right: 20rpx;
}

.tui-news-content {
	padding-top: 40rpx;
}

.tui-article {
	/* text-indent: 2em; */
	font-size: 34rpx;
	padding-bottom: 40rpx;
	line-height: 60rpx;
	text-align: justify;
	word-break: break-all;
	word-wrap: break-word;
}

.tui-article-pic {
	width: 100%;
	display: block;
	margin-bottom: 40rpx;
}

.tui-news-source {
	font-size: 24rpx;
	color: #999;
}

.tui-operate-box {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 60rpx 40rpx;
	box-sizing: border-box;
}

.tui-black {
	color: #333;
	padding-left: 12rpx;
}

.tui-cmt-title {
	font-size: 30rpx;
	font-weight: bold;
	position: relative;
}

.tui-cmt-title::after {
	content: '';
	position: absolute;
	left: -18rpx;
	top: 18%;
	width: 6rpx;
	height: 64%;
	background: #5677fc;
}

.tui-cmtbox {
	padding-bottom: 20rpx;
}

.tui-cmt-cell {
	display: flex;
	align-items: flex-start;
	justify-content: space-between;
	padding-top: 44rpx;
}

.tui-avatar {
	width: 64rpx;
	height: 64rpx;
	border-radius: 32rpx;
	display: block;
	flex-shrink: 0;
}

.tui-cmt-detail {
	width: 100%;
	padding-left: 16rpx;
	box-sizing: border-box;
}

.tui-header-box {
	display: flex;
	align-items: flex-start;
	justify-content: space-between;
	font-size: 30rpx;
}

.tui-cmt-nickname {
	color: #5677fc;
}

.tui-fabulous {
	color: #9a9a9a;
}

.tui-fabulous text {
	padding-right: 4rpx;
	font-size: 24rpx;
}

.tui-cmt-content {
	font-size: 32rpx;
	color: #333;
	text-align: justify;
	padding-top: 8rpx;
	word-break: break-all;
	word-wrap: break-word;
}

.tui-reply-box {
	border-radius: 8rpx;
	overflow: hidden;
	margin-top: 16rpx;
}

.tui-cell-last {
	display: flex;
	align-items: center;
	word-wrap: break-word;
	color: #5677fc;
}
.tui-flex-1 {
	flex: 1;
	width: 100%;
}

.tui-reply-nickname {
	font-size: 24rpx;
	color: #7a7a7a;
	padding-bottom: 8rpx;
}

.tui-footer {
	display: flex;
	align-items: center;
	font-size: 24rpx;
	margin-top: 16rpx;
	color: #9a9a9a;
}

.tui-primary {
	color: #5677fc !important;
}

.tui-ml {
	margin-left: 16rpx;
}

.tui-operation {
	width: 100%;
	height: 100rpx;
	overflow: hidden;
	background: #fff;
	display: flex;
	align-items: center;
	justify-content: space-between;
	position: fixed;
	left: 0;
	bottom: 0;
	z-index: 99999;
	padding-bottom: env(safe-area-inset-bottom);
}

.tui-safearea-bottom {
	width: 100%;
	height: env(safe-area-inset-bottom);
}

.tui-operation::before {
	content: '';
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	border-top: 1rpx solid #eaeef1;
	-webkit-transform: scaleY(0.5);
	transform: scaleY(0.5);
}

.tui-operation-left {
	display: flex;
	align-items: center;
}

.tui-operation-item {
	flex: 1;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	position: relative;
}

.tui-operation-right {
	height: 100rpx;
	box-sizing: border-box;
	padding-top: 0;
}

.tui-right-flex {
	display: flex;
	align-items: center;
	justify-content: center;
}

.tui-btn-comment {
	height: 64rpx;
	width: 84%;
	background: #ededed;
	color: #999;
	border-radius: 8rpx;
	font-size: 28rpx;
	display: flex;
	align-items: center;
	padding-left: 20rpx;
	box-sizing: border-box;
	padding-top: 0;
	margin-left: 30rpx;
}

.tui-col-7 {
	width: 58.33333333%;
}

.tui-col-5 {
	width: 41.66666667%;
}

.tui-share-btn {
	display: block;
	background: none;
	margin: 0;
	padding: 0;
}
</style>

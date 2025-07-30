<template>
	<view class="box animate-page">
		<zero-loading v-if="isLoading" type="wobble" position="absolute" mask color="#EB5A0E"></zero-loading>
		<view v-else style="display: flex;flex-direction: column;align-items: center;">
			<uni-nav-bar color="#EB5A0E" title="用户协议" left-icon="left" class="box-nav" fixed='true' shadow
				@clickLeft="tolastpage" />
			<view class="box-content">
				<view class="box-content-0">
					<text class="section-title">用户协议</text>
					<text class="date">(最新更新日期：2024年07月03日)</text>
					<text class="paragraph">
						欢迎使用【拾光】（以下简称“本App”或“我们”）。在您使用本App提供的服务之前，请仔细阅读以下条款。通过注册、下载、安装、访问或使用本App，即表示您已接受并同意受本《用户协议》的约束。如果您不同意本协议中的任何条款，请不要使用本App。
					</text>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第1条&nbsp;定义</text>
					<text class="paragraph">
						1.1&nbsp;"用户"是指使用本App的个人或实体。
						1.2&nbsp;"服务"是指本App提供的所有功能、内容及相关服务。
						1.3&nbsp;"内容"包括文字、图像、音频、视频、软件代码等所有信息材料。
					</text>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第2条&nbsp;账户与注册</text>
					<view class="paragraph1">
						<text>2.1&nbsp;您必须年满18岁或具有完全民事行为能力，方可注册使用本App。</text>
						<text>2.2&nbsp;注册时需提供真实、准确、完整的个人信息。</text>
						<view>2.3&nbsp;<text style="font-weight: 700;">您有责任妥善保管您的登录凭证，对因账号泄露造成的后果负责。</text></view>
						<text>2.4&nbsp;我们保留拒绝注册申请或注销账户的权利。</text>
					</view>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第3条&nbsp;服务内容与变更</text>
					<text class="paragraph">
						3.1&nbsp;本App提供以下服务：【请填写具体服务内容，如社交、购物、资讯、工具等】。
						3.2&nbsp;我们有权随时更改、暂停或终止部分或全部服务，恕不另行通知。
						3.3&nbsp;因技术维护、系统升级等原因，服务可能会临时中断，我们将尽量减少影响。
					</text>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第4条&nbsp;用户权利与义务</text>
					<view class="paragraph1">
						4.1&nbsp;您有权按照本协议约定使用本App的各项功能。
						4.2&nbsp;您不得从事以下行为：
						<view class="paragraph1-content">
							<text>(1)发布违法、虚假、侮辱性内容；</text>
							<text>(2)侵犯他人隐私权、知识产权或其他合法权益；</text>
							<text>(3)使用非法手段破解、篡改本App的功能；</text>
							<text>(4)进行垃圾信息群发、恶意刷单、刷榜等行为；</text>
							<text>(5)妨碍其他用户正常使用服务。</text>
						</view>
					</view>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第5条&nbsp;知识产权声明</text>
					<text class="paragraph">
						5.1&nbsp;本App中所有的内容、设计、图标、源码等均属于本公司或其授权方所有，受版权、商标、专利等相关法律保护。
						5.2&nbsp;未经书面许可，任何人不得复制、转载、修改、传播本App相关内容。
					</text>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第6条&nbsp;免责声明</text>
					<text class="paragraph">
						6.1&nbsp;本App提供的服务按“现状”提供，不作任何形式的明示或暗示的保证。
						6.2&nbsp;对于因使用或无法使用本App而产生的任何直接或间接损失，我们不承担责任。
						6.3&nbsp;第三方链接或广告内容不代表我们的观点或责任，用户自行判断风险。
					</text>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第7条&nbsp;隐私政策</text>
					<text class="paragraph">
						7.1&nbsp;我们非常重视您的隐私保护，关于信息收集、使用及保护，请参见《隐私政策》。
						7.2&nbsp;您继续使用本App，即表示您已阅读并同意我们的隐私政策。
					</text>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第8条&nbsp;协议的修改与终止</text>
					<view class="paragraph">
						8.1&nbsp;我们有权在必要时修改本协议内容，并通过App内公告或推送等方式通知您。
						8.2&nbsp;若您不同意修改后的条款，应立即停止使用本App。
						8.3&nbsp;如您违反本协议规定，我们有权单方面终止您的账户和服务。
					</view>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第9条&nbsp;适用法律与争议解决</text>
					<view class="paragraph">
						9.1&nbsp;
						<text style="font-weight: 700;">本协议适用中华人民共和国法律。
							因本协议引起的或与之相关的争议，双方应协商解决；协商不成的，任一方可向【公司地址】人民法院提起诉讼。</text>
					</view>
				</view>
				<view class="box-content-1">
					<text class="section-title1">第10条&nbsp;联系方式</text>
					<text class="paragraph">
						如有任何疑问或建议，请联系客服邮箱：【xxxxxxxxxxxx.com】
					</text>
				</view>
				<text>感谢您使用【拾光】！</text>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			isLoading: ''
		}
	},
	methods: {
		tolastpage() {
			uni.navigateBack()
		}
	},
	created() {
		this.isLoading = true
	},
	mounted() {
		setTimeout(() => {
			this.isLoading = false
		}, 300)
	}
}
</script>

<style scoped lang="less">
html,
body,
uni-page-body {
	margin: 0;
	background-color: #f8f8f8;
}

.box {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;

	&-nav {
		font-weight: 700;
	}

	&-content {
		width: 86%;
		height: auto;
		background-color: rgb(253, 253, 253);
		margin-top: 36rpx;
		font-size: 28rpx;
		padding: 20rpx;
		border-radius: 8rpx;
		box-shadow: 0 0 10rpx rgba(0, 0, 0, 0.2);
		margin-bottom: 36rpx;

		&-0 {
			display: flex;
			flex-direction: column;
		}

		&-1 {
			display: flex;
			flex-direction: column;
		}
	}
}

.section-title {
	font-size: 32rpx;
	font-weight: 700;
}

.section-title1 {
	font-size: 30rpx;
	font-weight: 700;
}

.date {
	font-size: 30rpx;
	text-indent: 1rem;
}

.paragraph {
	font-size: 30rpx;
	line-height: 1.3;
}

.paragraph1 {
	display: flex;
	flex-direction: column;
	font-size: 30rpx;

	&-content {
		display: flex;
		flex-direction: column;
		text-indent: 1rem;
	}
}

/* 页面间的过度动画 */
.animate-page {
	animation: pageSlideIn 0.8s ease;
}

@keyframes pageSlideIn {
	from {
		transform: translateX(-30rpx);
		opacity: 0;
	}

	to {
		transform: translateX(0);
		opacity: 1;
	}
}
</style>
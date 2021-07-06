<template>
	<view>
		<div class="pic-box-container">
			<div class="pic-box" v-for="(item,index) in imagePaths" :key="index">
				<div class="image" :style="{ backgroundImage:'url(' + item.tempFilePaths + ')', backgroundPosition:'center center', backgroundRepeat:'no-repeat', backgroundSize: 'cover' }"
				 @click="previewImg(item, index)">
				</div>
				<div class="close-btn" @click="removeImg(index)"></div>
			</div>
			<div class="pic-box" @click="pickImg" v-if="imagePaths.length < 9">
				<div class="add-box"></div>
			</div>
		</div>
	</view>
</template>

<script>
	export default {
		props: {
			imagePaths: {
				type: Array,
				default: function() {
					return [];
				}
			}
		},
		data() {
			return {};
		},
		methods: {
			pickImg() {
				let _that = this;
				uni.chooseImage({
					count: 1,
					sizeType: "original",
					success(res) {
						_that.imagePaths.push(res);
					}
				});
			},
			removeImg(index) {
				this.imagePaths.splice(index, 1);
			},
			previewImg(item, index) {
				uni.previewImage({
					urls: item.tempFilePaths,
					current: index
				});
			}
		}
	}
</script>

<style lang="scss">
	@import "@/styles/common.scss";

	$pic-box-width: 85px;

	$pic-box-default-bgcolor: #f0f0f0;

	.pic-box-container {
		align-items: flex-start;
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		border-radius: $pic-border-redius;

		.pic-box {
			border-radius: $pic-border-redius;
			margin-bottom: $common-font-size;
			width: $pic-box-width;
			height: $pic-box-width;
			background: $pic-box-default-bgcolor;
			margin-right: 15px;

			.image {
				width: 100%;
				height: 100%;
			}
		}

		$dot-width: 16px;

		.close-btn {
			width: $dot-width;
			height: $dot-width;
			text-align: center;
			border-radius: 50%;
			position: relative;
			right: calc(#{$pic-box-width} - #{$pic-box-width} * 2 + #{$dot-width} / 2);
			top: calc(#{$pic-box-width} - #{$pic-box-width} * 2 - #{$dot-width} / 2);
			background-image: url("@/static/delete-dot.png");
			background-size: 100% 100%;
		}

		.add-box {
			width: 100%;
			height: 100%;
			background-image: url("@/static/img-append.png");
			background-size: 100% 100%;
		}
	}
</style>

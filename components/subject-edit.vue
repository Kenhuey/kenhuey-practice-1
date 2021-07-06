<template name="SubjectEdit">
	<view class="content">
		<div class="conetnet-box">
			<div class="text-title">{{ title }}</div>
			<div class="text-content-container">
				<textarea autoHeight="true" rows="2" v-model="content.text" class="text-content" placeholder="null.content" :maxlength="maxLength"></textarea>
			</div>
			<img-picker :imagePaths="subjectImgRes"></img-picker>
			<div class="text-limit-container">{{ content.text.length }}/{{ maxLength }}</div>
		</div>
	</view>
</template>

<script>
	import ImgPicker from "@/components/img-picker.vue"

	export default {
		components: {
			ImgPicker
		},
		props: {
			title: {
				type: String,
				default: "null.title"
			},
			content: {
				default: ""
			},
			maxLength: {
				type: Number,
				default: 2048
			},
			subjectImgRes: {
				type: Array,
				default: function() {
					return [];
				}
			}
		}
	}
</script>

<style lang="scss" scope>
	@import "@/styles/common.scss";
</style>

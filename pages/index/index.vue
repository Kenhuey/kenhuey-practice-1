<template>
	<view class="content">
		<subject-edit title="题目内容(单选题)" :content="subjectContentText" :maxLength="3000" :subjectImgRes="subjectImgRes" />
		<selection-edit title="选项" :selections="selections.items" :selectedIndex="selections"></selection-edit>
		<subject-edit title="题目解析(选填)" :content="explanContentText" :maxLength="3000" :subjectImgRes="explanImgRes" />
		<!--
			这个按钮其实是可以用第三方组件库的 Affix(固钉) 去做效果更好
			奈何很少用 UniApp 不知道怎么引入组件库 :( 
		-->
		<div class="commit-button-place-holder"></div>
		<div class="commit-button-container">
			<div class="commit-button" @click="submit">保存</div>
		</div>
	</view>
</template>

<script>
	import SubjectEdit from "@/components/subject-edit.vue";
	import SelectionEdit from "@/components/selection-edit.vue"

	export default {
		components: {
			SubjectEdit,
			SelectionEdit
		},
		data() {
			let subjectContentText = {
				text: "测试测试测试11111"
			};
			let subjectImgRes = [];
			let explanContentText = {
				text: "测试测试测试22222"
			}
			let explanImgRes = [];
			let selections = {
				items: [{
					imagePaths: "",
					text: "测试11111"
				}],
				selectedIndex: 0
			};
			return {
				subjectContentText,
				subjectImgRes,
				selections,
				explanContentText,
				explanImgRes
			};
		},
		methods:{
			submit(){
				uni.showModal({
					title: "可能需要提交的数据",
					content: JSON.stringify( {
						subjectContentText: this.subjectContentText,
						subjectImgRes: this.subjectImgRes,
						explanContentText: this.explanContentText,
						explanImgRes: this.explanImgRes,
						selections: this.selections
					},null,4),
					showCancel: false
				})
			}
		}
	}
</script>

<style lang="scss" scope>
	@import "@/styles/common.scss";
	
	.commit-button-container{
		bottom: 0;
		position: fixed;
		width: 100%;
	}
	
	.commit-button{
		background: #15c19b;
		color: #ffffff;
		font-size: 14px;
		text-align: center;
		width: 80%;
		margin: 0 auto;
		margin-top: calc(#{$margin-offset} / 2);
		margin-bottom: calc(#{$margin-offset} / 2);
		line-height: calc(3 * 14px);
		border-radius: 28px;
	}
	
	.commit-button-place-holder{
		margin-top: calc(#{$margin-offset} / 2);
		margin-bottom: calc(#{$margin-offset} / 2);
		height: calc(3 * 14px);
		width: 100%;
	}
</style>

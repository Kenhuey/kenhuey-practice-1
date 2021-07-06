<template>
	<view class="content">
		<div class="conetnet-box">
			<div class="text-title">{{ title }}</div>
			<div class="selection-container" v-for="(item, index) in selectionsData" :keys="index">
				<div class="char-selection" :style="selectedStyle(index)" @click="selectIndex(index)">{{ charLetter[index] }}</div>
				<div class="input-container">
					<textarea 
						autoHeight="true"
						ref="textContent" 
						v-model="item.text" 
						placeholder="null.content" 
						class="input" 
					/>
					<div class="input-img">
						<div class="input-img-inner">
							<div class="input-img-inner-2" 
							:style="{ backgroundImage: 'url(' + item.imagePaths.tempFilePaths + ')', backgroundPosition: 'center center', backgroundRepeat: 'no-repeat', backgroundSize: 'cover' }"
							@click="pickImg(index)"
							@longpress="removeImg(index)"
							>
							</div>
						</div>
					</div>
				</div>
				<div class="selection-remove" @click="removeSelection(index)">
					<div class="selection-remove-icon"></div>
				</div>
			</div>
			<div class="selection-append" @click="appendSelection" v-if="selectionsData.length < 26">
				<div class="selection-append-icon"></div>
				添加选项
			</div>
		</div>
	</view>
</template>

<script>
	export default {
		props: {
			selections: {
				type: Array,
				default: function() {
					return [];
				}
			},
			title: {
				type: String,
				default: "null.title"
			},
			selectedIndex: {
				default: 0
			}
		},
		data() {
			let tempArr = [];
			for (let i = 97 - 32; i < 97 - 32 + 26; i++) {
				tempArr.push(String.fromCharCode(i));
			}
			const charLetter = tempArr;
			let selectionsData = this.selections;
			return {
				charLetter,
				selectionsData
			};
		},
		watch:{
			selections(val){
				this.selectionsData = val;
			}
		},
		methods:{
			selectIndex(index){
				this.selectedIndex.selectedIndex = index;
			},
			selectedStyle(index){
				if(index === this.selectedIndex.selectedIndex){
					return{
						background:"#15c19b",
						color:"#ffffff",
						border:"1px solid #15c19b"
					}
				}
				else return {}
			},
			pickImg(index){
				let _that = this;
				uni.chooseImage({
					count: 1,
					sizeType: "original",
					success(res) {
						_that.selections[index].imagePaths = res;
					}
				});
			},
			removeImg(index){
				this.selections[index].imagePaths = "";
			},
			removeSelection(index){
				if(index === this.selectedIndex.selectedIndex){
					this.selectedIndex.selectedIndex = -1;
				}
				this.selections.splice(index, 1);
			},
			appendSelection(){
				console.log(this.$refs.textContent);
				this.selections.push({
					imagePaths: "",
					text: "测试选项"
				});
			}
		}
	}
</script>

<style lang="scss" scope>
	@import "@/styles/common.scss";

	$selected-color: #15c19b;

	$selected-char-color: #506591;

	.selection-container {
		align-items: flex-start;
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		padding-top: calc(#{$content-font-size} / 2);
		padding-bottom: calc(#{$content-font-size} / 2);
		min-height: calc(3 * #{$content-font-size});
	}
	
	.input-container {
		background: #fbfbfb;
		border: 1px solid #eeeeee;
		border-radius: calc(#{$pic-border-redius} * 2);
		min-height: calc(3 * #{$content-font-size});
		width: calc(100% - 74px);
	}
	
	.input {
		float: left;
		font-size: $sub-font-size;
		line-height: calc(3 * #{$content-font-size});
		padding-left: $sub-font-size;
		width: calc(100% - 4 * #{$content-font-size});
		height: 100%;
		word-wrap : break-word;
		width: calc(100% - 44px);
	}
	
	.input-img {
		float: right;
		width: 32px;
		height: calc(3 * #{$content-font-size});
		line-height: calc(3 * #{$content-font-size});
		text-align: center;
	}
	
	.input-img-inner {
		$width: 20px;
		width: $width;
		height: $width;
		background-image: url("@/static/img-icon.png");
		background-size: 100% 100%;
		margin: 0 auto;
		margin-top: 10px;
	}
	
	.input-img-inner-2 {
		width: 100%;
		height: 100%;
	}
	
	.char-selection {
		margin-top: 7px;
		margin-right: 12px;
		width: calc(2 * #{$content-font-size});
		height: calc(2 * #{$content-font-size});
		border: 1px solid #ccd2df;
		text-align: center;
		color: #5e78ab;
		font-weight: 600;
		border-radius: calc(2 * #{$content-font-size});
		line-height: calc(2 * #{$content-font-size});
	}
	
	.selection-remove {
		margin-left: 12px;
		float: right;
		width: calc(2 * #{$content-font-size} - 6px);
		height: calc(3 * #{$content-font-size} + 2px);
	}
	
	.selection-remove-icon {
		width: calc(2 * #{$content-font-size} - 6px);
		height: calc(2 * #{$content-font-size} - 6px);
		background-image: url("@/static/group-edit-delete.png");
		background-size: 100% 100%;
		margin-top: calc((41px - 20px) / 2);
	}
	
	.selection-append{
		text-indent: $sub-font-size;
		padding-left: 40px;
		color: #15c19b;
		font-size: $sub-font-size;
		line-height: calc(3 * #{$content-font-size});
	}
	
	.selection-append-icon{
		float: left;
		background-image: url("@/static/group-edit-append.png");
		background-size: 100% 100%;
		width: calc(2 * #{$content-font-size} - 6px);
		height: calc(2 * #{$content-font-size} - 6px);
		margin-top: calc((41px - 22px) / 2);
	}
</style>

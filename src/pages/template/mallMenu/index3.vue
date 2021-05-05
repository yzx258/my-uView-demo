<template>
	<view class="u-wrap" style="font-family: Bahnschrift">
<!--		<view class="u-search-box">-->
<!--			<view class="u-search-inner">-->
<!--				<u-icon name="search" color="#909399" :size="28"></u-icon>-->
<!--				<text class="u-search-text">搜索</text>-->
<!--			</view>-->
<!--		</view>-->
<!--    <view class="wrap">-->
<!--      <u-swiper :list="list"></u-swiper>-->
<!--    </view>-->
    <view style="height:180px;background-color: #2b85e4;text-align: center">
      <view style="margin-top: 65px;font-size: 26px;color: white">
        <text>门店信息</text>
      </view>
    </view>
    <view style="padding: 10px">
      <u-row gutter="12">
        <u-col span="6">
          <view style="text-align: center">
            <text style="color:#2b85e4">销卖热点</text>
          </view>
        </u-col>
        <u-col span="6">
          <view style="text-align: center">
            <text style="color:#2b85e4">我的订单</text>
          </view>
        </u-col>
      </u-row>
    </view>
<!--    <view style="padding: 10px">-->
<!--      <u-row gutter="12">-->
<!--        <u-col span="4">-->
<!--          <view style="text-align: center">-->
<!--            <u-icon name="map"></u-icon><text>安溪小店</text>-->
<!--          </view>-->
<!--        </u-col>-->
<!--        <u-col span="4">-->
<!--          <view style="text-align: center">-->
<!--            <u-icon name="phone"></u-icon><text>15659512376</text>-->
<!--          </view>-->
<!--        </u-col>-->
<!--        <u-col span="4">-->
<!--          <view style="text-align: center">-->
<!--            <u-icon name="car"></u-icon><text>免运费</text>-->
<!--          </view>-->
<!--        </u-col>-->
<!--      </u-row>-->
<!--    </view>-->
		<view class="u-menu-wrap">
			<scroll-view scroll-y scroll-with-animation class="u-tab-view menu-scroll-view" :scroll-top="scrollTop"
			 :scroll-into-view="itemId">
				<view v-for="(item,index) in tabbar" :key="index" class="u-tab-item" :class="[current == index ? 'u-tab-item-active' : '']"
				 @tap.stop="swichMenu(index)">
					<text class="u-line-1">{{item.name}}</text>
				</view>
			</scroll-view>
			<scroll-view :scroll-top="scrollRightTop" scroll-y scroll-with-animation class="right-box" @scroll="rightScroll">
				<view class="page-view">
					<view class="class-item" :id="'item' + index" v-for="(item , index) in tabbar" :key="index">
						<view class="item-title">
							<text>默认商品列表</text>
						</view>
            <view>
              <view class="wrap u-row" v-for="(item1, index1) in item.foods" :key="index1">
                <u-row gutter="12">
                  <u-col span="4">
                    <view><img style="width: 120rpx;height: 180rpx"  :src="item1.url" mode=""></view>
                  </u-col>
                  <u-col span="8">
                    <view style="padding-top: 12rpx">{{item1.name}}</view>
                    <view style="color: red;padding-top: 12rpx"><u-icon name="rmb"></u-icon>{{item1.price}}</view>
                    <view style="color: #99a9bf;padding-top: 12rpx;font-size: 12px">还剩下{{item1.sum}}份</view>
                    <view style="float: right;">
                      <u-number-box :input-width="60" :input-height="60" v-model="value" @minus="updateShoppingCart(item1,0,index1)" @plus="updateShoppingCart(item1,1,index1)"></u-number-box>
                    </view>
                  </u-col>
                </u-row>
              </view>
            </view>
					</view>
				</view>
			</scroll-view>
		</view>
    <view>
      <u-popup v-model="show" mode="bottom" border-radius="14">
        <view class="content">
          <scroll-view scroll-y="true" style="height: 300rpx;">
            <view  style="height: 60px;margin-top: 10px" v-for="(item2, idx_1) in addShoppingCart" :key="idx_1">
              <u-row gutter="12">
                <u-col span="3">
                  <view  style="height: 60px;">
                    <img style="width: 50px;height: 60px;" :src=item2.url mode="">
                  </view>
                </u-col>
                <u-col span="4">
                  <view  style="height: 60px;font-size: 12px">
                    <view style="padding-top: 8rpx">{{ item2.name }}</view>
                    <view style="color: red;padding-top: 8rpx"><u-icon name="rmb"></u-icon>{{ item2.price }}</view>
                    <view style="color: #99a9bf;padding-top: 8rpx;font-size: 12px">还剩下{{ item2.sum }}份</view>
                  </view>
                </u-col>
                <u-col span="5">
                  <view  style="height: 60px;margin: 15px 15px">
<!--                    <u-number-box :input-width="60" :input-height="60" v-model="item2.value" @minus="updateShoppingCartSum(item2,0,idx_1)" @plus="updateShoppingCartSum(item2,1,idx_1)"></u-number-box>-->
                    <text>购买数量：{{ item2.value }}份</text>
                  </view>
                </u-col>
              </u-row>
            </view>

          </scroll-view>
          <view v-if="isShowDb" style="height: 100px;">
            <u-row gutter="12">
              <u-col span="3">
                <view style="width: 32px;height: 32px;position: relative;margin: 14px 14px;" @click="showCart">
                  <img style="width: 32px;height: 32px;" src="../../../static/购物车满.png" mode="">
                  <u-badge type="error" :count=shoppingCartSum offset="[-20,-20]"></u-badge>
                </view>
              </u-col>
              <u-col span="5">
                <view style="height: 32px;margin: 24px 14px">
                  <text>合计：{{ calculationAmountAll }} 元</text>
                </view>
              </u-col>
              <u-col span="4">
                <view style="margin-top: 10px;margin-right: 5px">
                  <u-button type="error">我已选好</u-button>
                </view>
              </u-col>
            </u-row>
          </view>
        </view>
      </u-popup>
      <view v-if="isShowDb" style="height: 100px;">
        <u-row gutter="12">
          <u-col span="3">
            <view style="width: 32px;height: 32px;position: relative;margin: 14px 14px;" @click="showCart">
              <img style="width: 32px;height: 32px;" src="../../../static/购物车满.png" mode="">
              <u-badge type="error" :count=shoppingCartSum offset="[-20,-20]"></u-badge>
            </view>
          </u-col>
          <u-col span="5">
            <view style="height: 32px;margin: 24px 14px">
              <text>合计：{{ calculationAmountAll }} 元</text>
            </view>
          </u-col>
          <u-col span="4">
            <view style="margin-top: 10px;margin-right: 5px">
              <u-button type="error">我已选好</u-button>
            </view>
          </u-col>
        </u-row>
      </view>
    </view>
	</view>
</template>
<script>
	// import classifyData from '@/common/classify.data.js';
	export default {
		data() {
			return {
        scrollTop: 0, //tab标题的滚动条位置
        oldScrollTop: 0,
        current: 0, // 预设当前项的值
        menuHeight: 0, // 左边菜单的高度
        menuItemHeight: 0, // 左边菜单item的高度
        itemId: '', // 栏目右边scroll-view用于滚动的id
        menuItemPos: [],
        arr: [],
        scrollRightTop: 0, // 右边栏目scroll-view的滚动条高度
        timer: null, // 定时器

        show: false,
        isShowDb:false,
        shoppingCartSum:0,
        addShoppingCart:[],
        calculationAmountAll:0,
        list: [{
          image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',
          title: '昨夜星辰昨夜风，画楼西畔桂堂东'
        },
          {
            image: 'https://cdn.uviewui.com/uview/swiper/2.jpg',
            title: '身无彩凤双飞翼，心有灵犀一点通'
          },
          {
            image: 'https://cdn.uviewui.com/uview/swiper/3.jpg',
            title: '谁念西风独自凉，萧萧黄叶闭疏窗，沉思往事立残阳'
          }
        ],

        tabbar: [{
          name: "女装",
          foods: [{
            id:"1",
            name: "气质白色长裙",
            price: "858.00",
            sum: "12",
            url: "https://cdn.uviewui.com/uview/common/classify/1/1.jpg",
            value:0
          }, {
            id:"2",
            name: "鲜艳白色长裙",
            price: "858.00",
            sum: "12",
            url: "https://cdn.uviewui.com/uview/common/classify/1/5.jpg",
            value:0
          },
            {
              id:"3",
              name: "A字裙",
              price: "858.00",
              url: "https://cdn.uviewui.com/uview/common/classify/1/7.jpg",
              sum: 10,
              value:0
            },
            {
              id:"4",
              name: "T恤",
              price: "858.00",
              url: "https://cdn.uviewui.com/uview/common/classify/1/2.jpg",
              sum: 10,
              value:0
            }
          ]
        }
        ]
			}
		},
		onLoad() {

		},
		onReady() {
			this.getMenuItemTop()
		},
		methods: {
      // 显示购物车
      showCart(){
        this.show = true;
      },
      // 改变数值
      updateShoppingCart(item1, index, arrIdx) {
        if (index == 1) {
          // 添加对象
          console.log(item1);
          this.isShowDb = true;
          if(undefined === this.addShoppingCart[arrIdx]){
            // 首次加入
            item1.value = item1.value + 1;
            this.addShoppingCart.push(item1);
          }else{
            // 已存在
            item1.value = item1.value + 1;
            this.addShoppingCart[arrIdx].value = item1.value;
          }
          this.shoppingCartSum = this.shoppingCartSum + 1;
        } else if (index == 0) {
          console.log(arrIdx)
          // 减少对象
          if(item1.value == 0){
            var index0 = this.addShoppingCart.findIndex(item => {
              if (item.id == item1.id) {
                return true
              }
            })
            this.addShoppingCart.splice(index0, 1);
          }
          this.shoppingCartSum = this.shoppingCartSum - 1;
        }
        this.calculationAmount(this.addShoppingCart);
        this.judgeShoppingCartSum();
      },
      // 改变数值
      updateShoppingCartSum(item1, index, arrIdx) {
        if (index == 1) {
          // 添加对象
          item1.value = item1.value + 1;
          this.addShoppingCart[arrIdx] = item1;
          this.shoppingCartSum = this.shoppingCartSum + 1;
        } else if (index == 0) {
          console.log(item1);
          var index0 = this.addShoppingCart.findIndex(item => {
            if (item.id == item1.id) {
              return true
            }
          })
          this.addShoppingCart.splice(index0, 1);
          // 减少对象
          this.shoppingCartSum = this.shoppingCartSum - 1;
        }
        this.calculationAmount(this.addShoppingCart);
        this.judgeShoppingCartSum();
      },
      // 计算金额
      calculationAmount(list) {
        let amount = 0;
        for (var index in list) {
          if(parseInt(list[index].value) > 0){
            amount = amount + parseFloat(list[index].price) * parseInt(list[index].value);
          }
        }
        this.calculationAmountAll = amount;
        console.log("我是金额：" + amount);
      },
      // 判断数量是否为0
      judgeShoppingCartSum(){
        if(this.shoppingCartSum == 0){
          this.show = false;
          this.isShowDb = false;
        }
      },
			// 点击左边的栏目切换
			async swichMenu(index) {
				if(this.arr.length == 0) {
					await this.getMenuItemTop();
				}
				if (index == this.current) return;
				this.scrollRightTop = this.oldScrollTop;
				this.$nextTick(function(){
					this.scrollRightTop = this.arr[index];
					this.current = index;
					this.leftMenuStatus(index);
				})
			},
			// 获取一个目标元素的高度
			getElRect(elClass, dataVal) {
				new Promise((resolve, reject) => {
					const query = uni.createSelectorQuery().in(this);
					query.select('.' + elClass).fields({
						size: true
					}, res => {
						// 如果节点尚未生成，res值为null，循环调用执行
						if (!res) {
							setTimeout(() => {
								this.getElRect(elClass);
							}, 10);
							return;
						}
						this[dataVal] = res.height;
						resolve();
					}).exec();
				})
			},
			// 观测元素相交状态
			async observer() {
				this.tabbar.map((val, index) => {
					let observer = uni.createIntersectionObserver(this);
					// 检测右边scroll-view的id为itemxx的元素与right-box的相交状态
					// 如果跟.right-box底部相交，就动态设置左边栏目的活动状态
					observer.relativeTo('.right-box', {
						top: 0
					}).observe('#item' + index, res => {
						if (res.intersectionRatio > 0) {
							let id = res.id.substring(4);
							this.leftMenuStatus(id);
						}
					})
				})
			},
			// 设置左边菜单的滚动状态
			async leftMenuStatus(index) {
				this.current = index;
				// 如果为0，意味着尚未初始化
				if (this.menuHeight == 0 || this.menuItemHeight == 0) {
					await this.getElRect('menu-scroll-view', 'menuHeight');
					await this.getElRect('u-tab-item', 'menuItemHeight');
				}
				// 将菜单活动item垂直居中
				this.scrollTop = index * this.menuItemHeight + this.menuItemHeight / 2 - this.menuHeight / 2;
			},
			// 获取右边菜单每个item到顶部的距离
			getMenuItemTop() {
				new Promise(resolve => {
					let selectorQuery = uni.createSelectorQuery();
					selectorQuery.selectAll('.class-item').boundingClientRect((rects) => {
						// 如果节点尚未生成，rects值为[](因为用selectAll，所以返回的是数组)，循环调用执行
						if(!rects.length) {
							setTimeout(() => {
								this.getMenuItemTop();
							}, 10);
							return ;
						}
						rects.forEach((rect) => {
							// 这里减去rects[0].top，是因为第一项顶部可能不是贴到导航栏(比如有个搜索框的情况)
							this.arr.push(rect.top - rects[0].top);
							resolve();
						})
					}).exec()
				})
			},
			// 右边菜单滚动
			async rightScroll(e) {
				this.oldScrollTop = e.detail.scrollTop;
				if(this.arr.length == 0) {
					await this.getMenuItemTop();
				}
				if(this.timer) return ;
				if(!this.menuHeight) {
					await this.getElRect('menu-scroll-view', 'menuHeight');
				}
				setTimeout(() => { // 节流
					this.timer = null;
					// scrollHeight为右边菜单垂直中点位置
					let scrollHeight = e.detail.scrollTop + this.menuHeight / 2;
					for (let i = 0; i < this.arr.length; i++) {
						let height1 = this.arr[i];
						let height2 = this.arr[i + 1];
						// 如果不存在height2，意味着数据循环已经到了最后一个，设置左边菜单为最后一项即可
						if (!height2 || scrollHeight >= height1 && scrollHeight < height2) {
							this.leftMenuStatus(i);
							return ;
						}
					}
				}, 10)
			}
		}
	}
</script>

<style lang="scss" scoped>

.content {
  padding: 24rpx;
  text-align: center;
}

    .u-row {
      margin: 15rpx 0;
    }

	.u-wrap {
		height: calc(100vh);
		/* #ifdef H5 */
		height: calc(100vh - var(--window-top));
		/* #endif */
		display: flex;
		flex-direction: column;
	}

	.u-search-box {
		padding: 18rpx 30rpx;
	}

	.u-menu-wrap {
		flex: 1;
		display: flex;
		overflow: hidden;
	}

	.u-search-inner {
		background-color: rgb(234, 234, 234);
		border-radius: 100rpx;
		display: flex;
		align-items: center;
		padding: 10rpx 16rpx;
	}

	.u-search-text {
		font-size: 26rpx;
		color: $u-tips-color;
		margin-left: 10rpx;
	}

	.u-tab-view {
		width: 200rpx;
		height: 100%;
	}

	.u-tab-item {
		height: 110rpx;
		background: #f6f6f6;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 26rpx;
		color: #444;
		font-weight: 400;
		line-height: 1;
	}

	.u-tab-item-active {
		position: relative;
		color: #000;
		font-size: 30rpx;
		font-weight: 600;
		background: #fff;
	}

	.u-tab-item-active::before {
		content: "";
		position: absolute;
		border-left: 4px solid $u-type-primary;
		height: 32rpx;
		left: 0;
		top: 39rpx;
	}

	.u-tab-view {
		height: 100%;
	}

	.right-box {
		background-color: rgb(250, 250, 250);
	}

	.page-view {
		padding: 16rpx;
	}

	.class-item {
		margin-bottom: 30rpx;
		background-color: #fff;
		padding: 16rpx;
		border-radius: 8rpx;
	}

	.class-item:last-child {
		min-height: 100vh;
	}

	.item-title {
		font-size: 26rpx;
		color: $u-main-color;
		font-weight: bold;
	}

	.item-menu-name {
		font-weight: normal;
		font-size: 24rpx;
		color: $u-main-color;
	}

	.item-container {
		display: flex;
		flex-wrap: wrap;
	}

	.thumb-box {
		width: 33.333333%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		margin-top: 20rpx;
	}

	.item-menu-image {
		width: 120rpx;
		height: 120rpx;
	}
</style>

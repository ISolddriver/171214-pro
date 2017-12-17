<template>
  <div>
  	<header class="header">
  		<div class="back iconfont">&#xe603;</div>
  		<div class="search">
        <div class="search-con">
          <i class="iconfont">&#xe6c6;</i>
          <span>输入城市/景点/游玩主题</span>
        </div>          
      </div>
  		<div class="city"><span class="city-con">城市</span><i class="iconfont">&#xe602;</i></div>
  	</header>

  	<swiper :options="swiperOption">
      <swiper-slide v-for="item in swiperInfo" :key="item.id">
        <div class="swiper-img-con">
          <img  class="swiper-img" :src="item.imgUrl"/>
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>

    <swiper :options="iconOption">
      <swiper-slide v-for="(pageInfo, index) in pages" :key="index">
      <div class="swiper-icons">
        <div v-for="item in pageInfo" :key="item.id" class="icon-item">
          <div class="icon-img-con">
            <img  class="icon-img" :src="item.imgUrl"/>
            <div class="icon-describe">{{item.keyword}}</div>
          </div>	
        </div>       
      </div>
      </swiper-slide>
      <p class="icon-pagination"  slot="pagination"></p>
    </swiper>

    <ul class="mp-listitem">
      <li class="listitem-con"><i class="mp-icon iconfont">&#xe604;</i>定位失败</li>
      <div class="one"></div>
      <li class="listitem-con"><i class="mp-icon iconfont">&#xe62c;</i>5折泡温泉</li>
    </ul>

    <ul class="mp-activity">
      <li class="mp-activity-con" v-for="item in activityInfo" :key="item.id"  :style="item.style">
        <a href="javascript:;"></a>
      </li>
    </ul>

    <div class="hot-sight">
        <h2 class="modtitle">热销推荐</h2>
        <div class="hot-con">
          <ul class="hot-ul">
            <li class="hot-list" v-for="item in hotInfo" :key="item.id">
              <div class="hot-img">
                <img :src="item.imgSrc">
              </div>
              <div class="hot-img-con">
                <div class="hot-title">{{item.title}}</div>
                <div class="hot-des">{{item.des}}</div>
                <div class="hot-price"><span class="yuan">￥</span><em class="hotprice">{{item.price}}</em>起</div>
              </div>
            </li>
          </ul>
          <div class="more">
            <a href="javascript:;" class="">查看所有产品</a>
          </div>          
        </div>
    </div>
    
    <div class="weekend-trip">
      <h2 class="modtitle">周末去哪儿</h2>
      <div class="trip-con" v-for="item in tripInfo" :key="item.id">
        <div class="trip-img">
          <img :src="item.url" :alt="item.title">
        </div>
        <div class="trip-des-con">
          <div class="trip-title">{{item.title}}</div>
          <div class="trip-des">{{item.des}}</div>         
        </div>
      </div>
    </div>
    
    <div class="price-con">
      <p><i class="iconfont">&#xe652;</i>是指通过景区指定窗口售卖的纸质门票上标注的价格</p>     
    </div>
    
    <div class="foot">
      <div class="footUL">
        <ul class="footlist">
          <li>
            <i class="iconfont">&#xe652;</i>机票
          </li>
          <li>
            <i class="iconfont">&#xe640;</i>酒店
          </li>
          <li>
            <i class="iconfont">&#xe656;</i>公寓
          </li>
          <li>
            <i class="iconfont">&#xe601;</i>更多
          </li>
          <ul class="person">
            <li>登录</li>
            <li>我的订单</li>
            <li>最近浏览</li>
            <li>关于我们</li>
            <li>个人中心</li>          
          </ul>
        </ul>
      </div>  
    </div>

  </div>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      tripInfo: [],
      hotInfo: [],
      activityInfo: [],
      swiperInfo: [],
      iconInfo: [],
      swiperOption: {
        autoplay: 1000,
        pagination: '.swiper-pagination',
        loop: 'true'
      },
      iconOption: {
        pagination: '.icon-pagination'
      }
    }
  },
  computed: {
    pages () {
      const pages = []
      this.iconInfo.forEach((value, index) => {
        let pageIndex = Math.floor(index / 8)
        if (!pages[pageIndex]) {
          pages[pageIndex] = []
        }
        pages[pageIndex].push(value)
      })
      return pages
    }
  },
  methods: {
    getData () {
      this.$http.get('./static/index.json')
      .then(this.handleGetDataSucc.bind(this))
    },
    handleGetDataSucc (res) {
      const body = res.body
      if (body && body.data && body.data.swiper) {
        this.swiperInfo = body.data.swiper
        this.iconInfo = body.data.icons
        this.activityInfo = body.data.activity
        this.hotInfo = body.data.hot
        this.tripInfo = body.data.trip
      }
    }
  },

  created () {
    this.getData()
  }
}
</script>

<style scoped>
	.header {
		display: flex;
		background: #05bad5;
		color: #fff; 
	}
	.back {
		width: 0.64rem;
		line-height: 0.86rem;
		text-align: center;
	}
	.search {
		flex: 1;
		margin: 0.14rem 0.18rem;
		background: #fff;
    border-radius: 0.1rem;
	}
  .search-con {
    margin-top: 0.14rem;
    margin-left: 0.2rem;
    color: #9e9e9e;
  }
	.city {
		width: 1.14rem;
		line-height: 0.86rem;
		text-align: center;
	}
	.swiper-img-con {
    overflow: hidden;
    width: 100%;
    height: 0;
    padding-bottom: 31.25%;
  }
  .swiper-img {
    width: 100%;
  }
  .icon-item {
    box-sizing: border-box;
    width: 25%;
    float: left;
    padding: 0.4rem;
    text-align: center;
  }
  .icon-img-con{ 
    height: 0;
    width: 100%;
    padding-bottom: 100%;
  }
  .icon-img {
    width: 70%;
  }
  .icon-describe {
    margin-top: 0.15rem;
    color: #212121;
    font-size: 0.26rem;
  }
  .icon-pagination {
    padding: 0 0.06rem;
    width: 100%;
    text-align: center;
    margin-bottom: 0.1rem;
  }
  .mp-listitem {
    display: flex;
    height: 0.98rem;
    border-top: 1px solid #9e9e9e;
  }
  /*.mp-listitem ::before {
    content: "";
    position: absolute;
    top: 0;
    height: 0.01rem;
    width: 100%;
    background: #999;
  }*/
  .listitem-con {
    width: 50%;    
    font-size: 0.28rem;
    color:#212121;
    line-height: 0.98rem;
    text-align: center;
    border-bottom: 0.01rem solid #9e9e9e;
  }
  .one {
    width: 1px;
    background: #999;
    height: 0.98rem;
  }
  .mp-activity {
    display: flex;
    height: 1.14rem;
    margin-top: 0.2rem;
    text-align: center;
    border-bottom: 1px solid #9e9e9e;
  }
  .mp-activity-con {
    width: 50%;
  }

  .modtitle {
    height: 0.8rem;
    color: #212121;
    line-height: 0.8rem;
    padding-left: 0.26rem;
  }
  .hot-con {
    width: 100%;
    margin-top: 0.2rem;
  }
  .hot-list {
    height: 1.4rem;
    padding: 0.15rem 0.15rem;
    border-bottom: 1px solid #9e9e9e;
  }
  .hot-img {
    width: 1.4rem;
    height: 1.4rem;
    float: left;
    margin: 0 0.2rem;
  }
  .hot-img>img {
    width: 100%;
  }
  .hot-img-con {
    width: 70%;
    height: 1.4rem;
    float: left;
  }
  .hot-des {
    height: 0.3rem;
  }
  .hot-title {
    font-size: 0.3rem;
    color: #212121;
  }
  .hot-des {
    font-size: 0.28rem;
    color: #9e9e9e;
    margin: 0.15rem 0;
  }
  .hotprice {
    font-size: 0.36rem;
    color: #ff8300;
  }
  .hot-price {
    font-size: 0.24rem;
    color: #9e9e9e;
  }
  .yuan {
    font-size: 0.24rem;
    color: #ff8300;
  }
  .more {
    height: 0.88rem;
    width: 100%;
    text-align: center;
    line-height: 0.88rem;
  }
  .more>a {
    color: #9e9e9e;
  }
  .trip-img {
    overflow: hidden;
    height: 0;
    width: 100%;
    padding-bottom: 37.4375%;
  }
  .trip-img>img {
    width: 100%;
  }
  .trip-des-con {
    padding: 0.14rem 0.2rem 0.2rem 0.2rem;
  }
  .trip-title {
    font-size: .28rem;
    line-height: .48rem;
  }
  .trip-des {
    font-size: .24rem;
    line-height: .42rem;
    color: #616161;
  }
  .price-con {
    font-size: 0.24rem;
    line-height: 0.32rem;
    padding: 0.14rem 0.1rem;
    color: #919191;
  }
  .footlist {
   
    width: 100%;
    height:31px;
    margin-left: .4rem;
  }
  .footlist>li {
    padding-left: 0.4rem;
    line-height: 0.48rem;
    height: 31px;
    width: 65px;
    float: left;
  }
  .person {
    
    height: 31px;
    width: 100%;
  }
  .person>li {
    padding-left: 0.25rem;
    line-height: 0.48rem;
    height: 31px;
    float: left;
  }
</style>
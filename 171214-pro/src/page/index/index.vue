<template>
  <div>
  	<header class="header">
  		<div class="back iconfont">&#xe625;</div>
  		<div class="search"></div>
  		<div class="city">城市</div>
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
      <li class="listitem-con">定位失败</li>
      <div class="one"></div>
      <li class="listitem-con">5折泡温泉</li>
    </ul>
  </div>
  
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
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
    width: 86%;
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
    height: 0;
    border-top: 1px solid #999;
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
    height: 0.98rem;
    font-size: 0.28rem;
    color:#212121;
    line-height: 0.98rem;
    text-align: center;
    border-bottom: 0.01rem solid #999;
  }
  .one {
    width: 1px;
    background: #999;
    height: 0.98rem;
  }
</style>
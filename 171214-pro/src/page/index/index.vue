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

    <swiper :options="{}">
      <swiper-slide v-for="(pageInfo, index) in pages" :key="index">
      <div v-for="item in pageInfo" :key="item.id">
        <div class="swiper-icon-con">
          <img  class="swiper-icon" :src="item.imgUrl"/>
        </div>	
      </div>
      </swiper-slide>
    </swiper>

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
      }
    }
  },
  computed: {
    pages () {
      const pages = []
      this.iconInfo.forEach((value, index) => {
        let pageIndex = Math.floor(index/8)
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
</style>
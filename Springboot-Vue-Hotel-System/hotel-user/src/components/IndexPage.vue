<template>
  <div class="hello">
        <mu-paper class="banner">
          <img src="../assets/logo26.png" class="logo">
          <h1>{{ hotelInfo.hotelName }}</h1>
          <h2>一家国际化连锁经营的大型酒店</h2>
          <p v-if="!isLogin()">
            <mu-button color="orange" @click="navigateTo('/login')">
              <b>立即登录</b>
            </mu-button>
            <mu-button color="success" @click="navigateTo('/register')">立即注册</mu-button>
          </p>
          <p v-else="isLogin()">
            <mu-button color="secondary" @click="navigateTo('/room')">
            浏览热门客房
            </mu-button>
            <mu-button color="" @click="navigateTo('/order')">查看个人订单</mu-button>
          </p>
        </mu-paper>
    <!-- -->

    <!---->
    <!---->
    <!---->
    <div class="footer">
      Copyright @ <b> {{ hotelInfo.hotelName }} </b><br>
      地址：<b> {{ hotelInfo.address }} </b><br>
      电话号码： <b> {{ hotelInfo.telephone }} </b><br>
      电子邮箱：<b> {{ hotelInfo.email }} </b><br>
      网页地址：<b> {{ hotelInfo.website }} </b>
    </div>
  </div>
</template>

<script>
  import indexImg1 from '../assets/imgs/index1.png'
  import indexImg2 from '../assets/imgs/index2.png'
  import indexImg3 from '../assets/imgs/index3.png'
  import colImg from '../assets/banner/banner2.jpg'
  import Cookies from 'js-cookie'
  import { getAllHotel } from "@/api/hotel"

  export default {
  name: 'IndexPage',
  data () {
    return {
      indexImg1,
      indexImg2,
      indexImg3,
      hotelInfo: {
        hotelName: "ho",
        address: ""
      },
      list: [{
        image: colImg,
        title: 'Breakfast',
        author: 'Myron'
      }, {
        image: colImg,
        title: 'Burger',
        author: 'Linyu'
      }, {
        image: colImg,
        title: 'Camera',
        author: 'ruolin'
      }]
    }
  },
  created: function() {
    this.isLogin()
    this.fetchData()
  },
  methods:{
    isLogin() {
      const username = Cookies.get("username")
      if (username == null || username === '') {
        return false;
      }
      else {
        return true
      }
    },
    navigateTo(val) {
      this.$router.push(val);
    },
    fetchData(){
      getAllHotel().then(res => {
        this.hotelInfo = res.data[0]
      }).catch(err => {
        console.log(err)
        this.$toast.error(err.toString())
      })
    },

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.logo{
  border-radius: 50%;
  width: 100px;
  height: 100px;
}
  .banner {
    /*margin-top: 64px;*/
    display: flex;
    flex-direction: column;
    /*background-image: linear-gradient(270deg,#8146b4,#6990f6);*/
    background-color: #add8e6; /* 浅蓝色背景 */
    height: 600px;
    align-items: center;
    justify-content: center;
    color: #fff;
  }
.carousel img{
    width: inherit;
  }
.demo-paper,.hello{
  text-align: center;
}
  .demo-paper img{
    width: 300px;
  }
  .footer{
    text-align: center;
    color: #fff;
    padding: 50px 40px;
    background-color: #add8e6;
    /*background-image: linear-gradient(270deg,#8146b4,#6990f6);*/
  }
.gridlist-inline-demo {

  display: flex;
  flex-wrap: nowrap;
  overflow-x: auto;
}
</style>

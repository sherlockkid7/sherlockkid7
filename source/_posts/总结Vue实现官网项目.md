---
title: 总结Vue实现官网项目
date: 2021-05-10 21:42:25
tags: 随记
categories: 工作
---

最近使用vue写了一个官网项目，总结一下遇到的一些问题。（遇到好些坑，每个都花费了大把时间才解决掉，技术菜的一批）

##### 1. 使用swiper(vue-awesome-swiper)实现滚屏展示页面

注意点：

###### a.最外层的swiper框不要写同级元素，不然会出现滚动条

###### b.自定义指示点的使用

```js
//写在data函数中
swiperVertical: {
        direction: 'vertical',
        slidesPerView: 1,
        mousewheel: true,
        height: window.innerHeight, // 高度设置，占满设备高度
        observer: true,
        observeParents: true,
        //自定义指示点
        pagination: {
          el: '.vertical-pagination',
          clickable: true,
          renderBullet: function (index, className) {
            switch(index){
              case 0:text='TOP';break;
              case 1:text='简介';break;
              case 2:text='方案';break;
              case 3:text='服务';break;
              case 4:text='新闻';break;
              case 5:text='分布';break;
              case 6:text='历程';break;
              case 7:text='END';break;
            }
            return '<span class="' + className + '">' + text + '</span>';
          },
        },
        navigation: {
          nextEl: '.vertical-next',
        },
        on: {
          slideChange: function(){
            homeIndex = this.activeIndex;
          },
          slideChangeTransitionStart: function(){
            if(this.activeIndex === 7){
              this.navigation.$nextEl.css('display','none');
            }else{
              this.navigation.$nextEl.css('display','block');  
            } 
          },
          slideChangeTransitionEnd: function(){
            
          },
        },
      },
```

###### c.页面在第二屏的时候，实现数字从0到固定值的增长

【使用插件 animated-number-vue】

```html
<Animated-number
:value="this.value1"
:formatValue="formatToSum"
:duration="1000"
/>
```

```js
import AnimatedNumber from "animated-number-vue";
export default {
  name: 'Home',
  components: {
    AnimatedNumber
  },
  data() {
      return{
          value1:0,
          value2:0,
          value3:0,
          value4:0,
      }
  },
  methods:{
      //slideChangeTransitionEnd方法是绑定在最外层swiper的，表示从一个slide过渡到另一个slide结束时执行的回调函数
      slideChangeTransitionEnd(){
          if(homeIndex === 1){
            // 实现数字增长
            this.value1 = Number(0) + 200;
            this.value2 = Number(0) + 3000;
            this.value3 = Number(0) + 15000;
            this.value4 = Number(0) + 1000000;
          }else{
            this.value1 = 0
            this.value2 = 0
            this.value3 = 0
            this.value4 = 0
          }
      }
  }
}
```

d.滚动加载数据或者点击指示点加载数据，对于已经加载过的数据不要再次加载

```js
// 两种方法，使用的第一种
// 1.arr状态  let loadArr = [0,1,2,3,4,5,6,7]   根据每一屏的arr是否等于1，来确定是否加载(此变量不能定义在全局)
// 2.查询数据是否存在

//写在事件中
onSwiperSlideChange(){
    if(homeIndex === 1 && loadArr[2] != 1){
        this.getSchemedata(2)
    }else if(homeIndex === 2 && loadArr[3] != 1){
        this.getServicedata(3)
    }else if(homeIndex === 3 && loadArr[4] != 1){
        this.getNewsdata1()
        this.getNewsdata2()
        this.getNewsdata3(4)

    }else if(homeIndex === 4 && loadArr[5] != 1){
        this.getDistrdata(5)
    }else if(homeIndex === 5 && loadArr[6] != 1){
        this.getHistorydata(6)
    }else if(homeIndex === 6 && loadArr[7] != 1){
        this.getEnddata(7)
    }
},
    
//请求的数据
getSchemedata(index){
    let schemeArr = [58,59,60]
    let schemeStr = schemeArr.toString()
    getData({ids:schemeStr}).then(res => {

        this.schemeContent = res.data[0]
        this.schemePanel1 = res.data[1]
        this.schemeSource = JSON.parse(res.data[1].picUrl)
        this.schemePanel2 = res.data[2]
        loadArr[index]=1;
    })
},
```

c.特殊的轮播展示。4张图片，中间两个激活状态，两边呈现灰色蒙层【自己写的始终存在问题，后面再同事的帮助下实现了】。

```vue
<template>
  <div class="news-swiper" v-swiper:mySwiper="newsOption">
    <div class="swiper-wrapper" ref="news">
      <div class="swiper-slide news-slides" v-for="(item,index) in newsData" :key="index" 
        @click="goDetails(item.id,item.menuId)" :class="{'active':index==swiActiveIndex+1 || index==swiActiveIndex+2}">
        <div class="pic-box">
          <img :src="item.cover" class="object-cover">
        </div>
        <div class="desc">
          <div class="title overflow">{{item.name}}</div>
          <p class="date">{{item.createTime | formatDate}}</p>
        </div>
      </div>
    </div>
    <!-- @click="subIndex"
    @click="addIndex" -->
    <div class="swiper-button-prev" >
      <img src="~/assets/img/arrow-left.png" alt="">
    </div>
    <div class="swiper-button-next">
      <img src="~/assets/img/arrow-right.png" alt="">
    </div>
  </div>
</template>

<script>
  let index,slides
  import { formatDate } from 'common/date.js';
  export default {
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd');
      }
    },
    props:{
      newsData:Array, 
    },
    data () {
      let self = this;
      return {
        swiActiveIndex:'',
        newsOption:  {
          slidesPerView: 4,
          spaceBetween: 40,
          // centeredSlides : true,
          // slidesPerGroup: 4,
          // loop: true,
          observer:true,
          observeParents:true,
          allowTouchMove:false,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
          },
          on: {
            init: function(){
              index = this.activeIndex; 
            },
            slideChange: function(){
              let that = this;
              index = this.activeIndex;
              self.swiActiveIndex = this.activeIndex;
            },
          },
				}
      } 
    },
    watch:{
      // newsData(){
      //   if(this.$refs.news){
      //     this.$nextTick(() => {
      //       slides = this.$refs.news.children
      //       slides[index+1].classList.add('active')
      //       slides[index+2].classList.add('active')
      //     })
      //   }
      // },

      
    },
    mounted() {
      // this.changeStyle()
    }, 
    
    methods:{
      // addIndex() {
      //   for(var i = 0;i<slides.length;i++){
      //     slides[i].classList.remove('active')
      //   }
      //   slides[index+2].classList.add('active')
      //   slides[index+3].classList.add('active')
      // },
      // subIndex(){
      //   for(var i = 0;i<slides.length;i++){
      //     slides[i].classList.remove('active')
      //   }
      //   slides[index+1].classList.add('active')
      //   slides[index].classList.add('active')
      // },
      
      
      goDetails(id,menuId) {
        this.$router.push({
          path: `/news/details/${menuId}/${id}`,
        })
      }
    }
  }
</script>
<style lang="less" scoped>
  .news-swiper{
    height: 100%;
    .swiper-slide{
      display: flex;
      flex-direction: column;
      width: 580px;
      cursor: pointer;
      .pic-box{
        position: relative;
        width: 100%;
        height: 322px;
        opacity:0.4;
        // &::after{
        //   content: '';
        //   position: absolute;
        //   top: 50%;
        //   left: 50%;
        //   transform: translate(-50%,-50%);
        //   width: 100%;
        //   height: 100%;
        //   background: rgba(255, 255, 255, .7);
        // }
      }
      .desc{
        display: none;
        color: rgba(0, 0, 0, 0.85);
        .title{
          font-size: 18px;
          font-weight: bold;
          
          margin: 17px 0;
        }
        .date{
          font-weight: bold;
        }
      }
    }
    // .swiper-slide-active{
    //   .desc{
    //     display: block;
    //   }
    //   .pic-box{
    //     &::after{
    //       background: transparent;
    //     }
    //   }
    // }
    .active{
      .desc{
        display: block;
      }
      .pic-box{
        opacity: 1;
        // &::after{
        //   background: transparent;
        // }
      }
    }
    .swiper-button-prev{
      width: 74px;
      height: 74px;
      opacity: 1;
      top:30%;
      left: 23.6%;
      z-index: 1000;
      cursor: pointer;
    }
    .swiper-button-next{
      width: 74px;
      height: 74px;
      opacity: 1;
      top:30%;
      right:23.6%;
      z-index:1000;
      cursor: pointer;
    }
  }
  @media screen and  (max-width:1460px){
  .news-swiper .swiper-slide .pic-box{
    height: 200px;
  }
  }
</style>
```

##### 2.背景图的展示问题

注：如果后台返回的url中存在空格或者汉字，使用:style绑定背景图为undefined

##### 3.获取浏览器名称、版本、ip[通过使用搜狐的api获取访问ip]

在index.html文件中添加

```js
<script src="http://pv.sohu.com/cityjson?ie=utf-8"></script>
```

在组件中使用

```js
//获取浏览器信息
getBrowserInfo() {
    var agent = navigator.userAgent.toLowerCase()

    var regStr_ie = /msie [\d.]+;/gi
    var regStr_ff = /firefox\/[\d.]+/gi
    var regStr_chrome = /chrome\/[\d.]+/gi
    var regStr_saf = /safari\/[\d.]+/gi
    //IE
    if (agent.indexOf('msie') > 0) {
        return agent.match(regStr_ie)
    }

    //firefox
    if (agent.indexOf('firefox') > 0) {
        return agent.match(regStr_ff)
    }

    //Safari
    if (agent.indexOf('safari') > 0 && agent.indexOf('chrome') < 0) {
        return agent.match(regStr_saf)
    }

    //Chrome
    if (agent.indexOf('chrome') > 0) {
        return agent.match(regStr_chrome)
    }
},
    getPageAccessRecord() {
        var browser = this.getBrowserInfo()
        if(browser) {
            browser = browser.toString()
        }
        var verinfo = (browser + '').replace(/[^0-9.]/gi, '')
        var cip = returnCitySN["cip"];

        const params = {
            appType: ' jituanWeb',
            menuId: 'home',
            browserName: browser,
            browserVersion: verinfo,
            ip: cip
        }
        console.log(params);
        getRecord(params).then(res => {
            console.log(res);
        })
    },
```



##### 4.从一个页面的链接跳转到另一个页面的指定位置

使用router-link实现页面跳转，并传递参数（相对应的索引值）

```html
// A页面
<div class="show-box">
    <div class="show-item transition"  
         v-for="(item,index) in historyList"
         :key="index" 
         v-show="index===number" >
        <div class="content">
            <h3>{{ item.title }}</h3>
            <p> {{ item.list[0].content }}</p>
        </div>
        <router-link :to="{path:`about?tab${index+1}`}" class="watch-more">查看更多 <img src="~/assets/img/watch-more.png" alt=""></router-link>  
    </div>
    <div class="arrow-left"
         @click="reduceNum(number)">
        <img src="~/assets/img/arrow-left.png" alt="">
    </div>
    <div class="arrow-right"           
         @click="addNum(number)">
        <img src="~/assets/img/arrow-right.png" alt="">
    </div>
</div>



```

```js
//B页面
<script>
import { getData,getAboutHistory,getRecord } from 'network/api'

import {goAnchor} from 'common/common';
export default {
  name: 'About',
  data() {
    return {
      number:5,
  },
  
  mounted() {
     //得到url的hash值
     const hash = window.location.hash;
     let index = hash.lastIndexOf("?");
     if (index != -1) {
        // 获取传递的参数
        let idEle = hash.substring(index + 1, hash.length + 1);
        //根据参数的索引改变此页面对应的tab
        this.number = parseInt(idEle.charAt(idEle.length - 1)) - 1;
        
        setTimeout(() => {
            this.goAnchor(idEle)
          
        },800)
      }
 },
```

```js
//common.js
function goAnchor(selector) {
  this.$nextTick(function() {
    var anchor = document.getElementById(selector);//获取元素   // 获取到div元素
    
    if(anchor) {
        setTimeout(()=>{//页面没有渲染完成时是无法滚动的，因此设置延迟        
//          var targetOffset = anchor.offsetTop;
//          console.log(targetOffset); 
            var body = document.body
            body.scrollTop = 2800
        });

    } 
})
};

export {
    goAnchor, 
}
```



##### 5.表单验证(使用element实现)

```html
<div class="form-box">
      <div class="fill-box">
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" >
          <h3>
            申请加盟小来
          </h3>
          <el-form-item label="姓名：" prop="name">
            <el-input type="name" v-model="ruleForm.name"></el-input>
          </el-form-item>
          <el-form-item label="手机：" prop="phone">
            <el-input v-model.number="ruleForm.phone"></el-input>
          </el-form-item>
          <el-form-item label="加盟项目：" prop="program" class="spe-item">
            <el-select v-model="ruleForm.program" multiple>
              <el-option
                v-for="item in programsData"
                :key="item.dictCode"
                :label="item.dictLabel"
                :value="item.dictCode">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="微信：（选填）" prop="wx">
            <el-input v-model="ruleForm.wx"></el-input>
          </el-form-item>
          <div class="code">
            <el-form-item label="验证码" prop="code" class="code-box">
              <el-input v-model="ruleForm.code"></el-input>
            </el-form-item>
            <div class="verify" @click="changeCode" v-if="code">
              <img :src="'data:image/gif;base64,' + code.img" alt="" class="object-cover">
            </div>
          </div>
          <el-button type="primary" @click="submitForm()">提交</el-button>
        </el-form>
      </div>
    </div>
```



```js
<script>
import { getData,getJoin,getCode,getRecord,getJoinProgram } from 'network/api'

export default {
  
  data () {
    var checkname = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('请输入姓名'));
      }else{
        callback();
      }
    };
    var mPattern = /^1[3456789]\d{9}$/;
    var validatePone = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入手机号'));
      } else if (!mPattern.test(value)) {
        callback(new Error('手机格式错误'));
      } else {
        callback();
      }
    };
    
    var  validateCode = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('请输入验证码'));
      }else{
        callback();
      }
    };
    return {
      ruleForm: {
        name: '',
        phone: '',
        wx: '',
        code:'',
        uuid:'',
        program:''
      },
      rules: {
        name: [
          { validator: checkname, trigger: 'blur' }
        ],
        phone: [
          { validator: validatePone, trigger: 'blur' }
        ],
        code:[
          { validator: validateCode,  trigger: 'blur' }
        ],
        program:[
          { required: true, message: '请选择加盟项目', trigger: 'blur' }
        ],
      },

      code:'',
      currentPage: 1,
      pageSize: 50,
      programsData: ''
    }
    
  },
  created() {
    this.getCodeData()
    this.getJoinProgramData()
  },
  methods: {
    getCodeData() {
      getCode().then(res => {
        this.code = res
        this.ruleForm.uuid = res.uuid
      })
    },
    getJoinProgramData() {
      let params = {
        pageNum: this.currentPage,
        pageSize: this.pageSize,
        dictType: 'joinProgram'
        
      }
      getJoinProgram(params).then(res => {
        this.programsData = res.rows
      })
    },
    changeCode(){
      this.getCodeData()
    },
    //表单提交【提交成功后再清空表单数据】
    submitForm() {
      this.$refs.ruleForm.validate((res) => {
        if(res){
          let data = {
            name: this.ruleForm.name,
            phoneNo: this.ruleForm.phone,
            wxNo: this.ruleForm.wx,
            code: this.ruleForm.code,
            uuid: this.ruleForm.uuid,
            joinProgram: this.ruleForm.program.toString(),
          };
          console.log(data);
          getJoin(data).then(res => {
            
            if(res.code == 200){
                this.$message({
                  message: res.msg,
                  type: 'success',
                  offset:100
                });
                this.ruleForm = {};
              }else{
                this.$message({
                  message: res.msg,
                  type: 'warning',
                  offset:100
                });
                
                this.getCodeData()
              }
          })
        }
      });
      
    }
  }
}
</script>
```










<template>
   <div class="daily">
        <div class="daily-menu">
            <div class="daily-menu-item" 
            :class="{on:type==='recommend'}" 
            @click="handlechangeThemes('recommend')">每日主题</div>
            <div class="daily-menu-item" 
            :class="{on:type==='daily'}">主题日报</div>
            <ul>
                <li v-for="item in themes" :key="item.id">
                        <a :class="{on:item.id==themeId&&type==='daily'}"
                        @click="handlechangeThemes('daily')">{{item.name}}</a>
                </li>
            </ul>
        </div>
        <div class="daily-list">
            <template>
                <div v-for="list in recommendList">
                     <div class="daily-date">{{ formatDay(list.date) }}</div>
                   <Item  v-for="item in list.stories"
                    :data='item'
                    :key="item.id"
                    @click.native="handleRecommend(item.id)"></Item>
                </div>
            </template>
        </div>
        <daily-article :id="articleId"></daily-article>
   </div>
</template>
<script>
   import Item from './components/item.vue';
   import dailyArticle from './components/daily-article.vue';
   import $ from './lib/util.js'

      export default{
          components:{
            Item,
            dailyArticle
          },
          data(){
              return {
                  themes:[
                      {
                        'name':'日常心里',
                        'id':13,
                        'thumbnail':'http://pic3.zhimg.com/xxx.jpg',
                        'color':'15007',
                        'description':'了解自己和别人，了解彼此的欲望和局限'
                      }
                  ],
                  showThemes:true,
                  type:'recommend',
                  themeId:0,
                  recommendList:[],
                  isLoading:false,
                  dailyTime: $.getTodayTime(),
                  articleId:0,
                  id:0
              }
          },
          methods:{
              getThemes(){
                  //axios 发起get 请求
                //   $.ajax.get('themes').then(res=>{
                //       this.themes=res.others;
                //   })
              },
              //切换主题
              handlechangeThemes(themeName){
                  this.type=themeName;
              },
              //获取title
              getRecommendList(){
                  this.isLoading=true;
                  const prevDay=$.prevDay(this.dailyTime+86400000);
                  $.ajax.get('news/before/'+prevDay).then(res=>{
                      this.recommendList.push(res);
                      this.isLoading=false;
                  });
              },
               formatDay (date) {
                let month = date.substr(4, 2);
                let day = date.substr(6, 2);
                if (month.substr(0, 1) === '0') month = month.substr(1, 1);
                if (day.substr(0, 1) === '0') day = day.substr(1, 1);
                return `${month} 月 ${day} 日`;
            },
            //点击title
            handleRecommend(articleId){
                console.log(articleId);
                this.articleId=articleId
            }
          },
          mounted(){
              //初始化时调用
              this.getThemes();
              //初始化title
              this.getRecommendList();
          }
      }
</script>

<style>
    html,body{
        margin: 0;
        padding: 0px;
        height: 100%;
        color: #657180;
        font-size: 16px;
    }

    .daily-menu{
       width: 150px;
       position: fixed;
       top:0;
       bottom: 0;
       left: 0;
       overflow: auto;
       background-color: #f5f7f9;
    }

    .daily-menu-item{
        font-size: 18px;
        text-align: center;
        margin: 5px 0px;
        padding: 10px 0px;
        cursor: pointer;
        border-right: 2px solid transparent;
        transition: all .3s ease-in-out
    }

      .daily-menu-item:hover{
            background-color: #e3e8ee
      }

       .daily-menu-item.on{
          border-right: 2px solid #3399ff;
       }

       .daily-menu ul{
           list-style: none;
       }

       .daily-menu ul li a{
           /* display: none; */
           color: inherit;
           text-decoration: none;
           padding: 5px 0;
           margin: 5px 0;
           cursor: pointer;
       }

       .daily-menu ul li a:hover,.daily-menu ul li a.on{
           color: #3399ff;
       }

       .daily-list{
           width: 300px;
           position: fixed;
           top: 0;
           bottom: 0;
           left: 150px;
           overflow: auto;
           border-right: 1px solid #d7dde4;
       }
      
       .daily-item{
           display: block;
           color:inherit;
           text-decoration: none;
           padding: 16px;
           overflow: hidden;
           cursor: pointer;
           transition: all .3s ease-in-out;
       }
       
       .daily-item:hover{
           background-color: #e3e8ee;
       }
       .daily-img{
           width:80px;
           height: 80px;
           float: left;
       }

       .daily-img img{
           width:100%;
           height: 100%;
           border-radius: 3px;
       }
       .daily-title{
           padding: 10px 5px 10px 90px;
       } 

       .daily-title.noImg{
           padding-left: 5px;
       }

       .daily-article{
           margin-left: 450px;
           padding: 20px;
       }
       
       .daily-article-title{
           font-size: 28px;
           font-weight: bold;
           color: #222;
           padding: 10px 0;
       }
       .view-more a{
           display: block;
           cursor: pointer;
           background-color: #f5f7f9;
           text-align: center;
           color: inherit;
           text-decoration: none;
           padding: 4px 0;
           border-radius: 3px;
       }

       .daily-comments{
           margin: 10px 0;
       }

       .daily-comments span{
           display: block;
           margin: 10px 0;
           font-size: 20px;
       }

       .daily-comment{
           overflow: hidden;
           margin-bottom: 20px;
           padding-bottom: 20px;
           border-bottom: 1px dashed #e3e8ee;
       }

       .daily-comment-avatar{
           width: 50px;
           height: 50px;
           float: left;
       }

       .daily-comment-avatar img{
           width:100%;
           height: 100%;
           border-radius: 3px;
       }   

       .daily-comment-content{
           margin-left: 65px;
       }

       .daily-comment-name{

       }

       .daily-comment-time{
           color: #e9a7b4;
           font-size: 14px;
           margin-top: 5px;
       }

       .daily-comment-text{
           margin-top: 10px;
       }
</style>

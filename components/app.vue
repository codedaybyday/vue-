<template>
    <div>
        <div class="faces">
            <a href="" class="fl"><img :src="left.img"></a>
            <a href="" class="fr"><img :src="right.img"></a>
        </div>
        <div class="recipe-pk-detail">
            <ul>
                <li class="fl">
                    <dl>
                        <dd class="rate left-rate">{{left.rate*100+'%'}}</dd>
                        <dd class="recipe-num">{{left.dish_num}}人做了{{left.dish_name}}</dd>
                    </dl>
                </li>
                <li class="fr">
                    <dl>
                        <dd class="rate right-rate">{{right.rate*100+'%'}}</dd>
                        <dd class="recipe-num right-align">{{right.dish_num}}人做了{{right.dish_name}}</dd>
                    </dl>
                </li>
            </ul>
            <div class="pk-desc">
                {{des}}
            </div>
        </div>
        <ul class="pk-tab-wrap">
            <li class="pk-tab fl"><span :class="{active:index==0}" @click="toggle">{{left.dish_name}}</span></li>
            <li class="pk-tab fr"><span :class="{active:index==1}" @click="toggle">{{right.dish_name}}</span></li>
        </ul>
        <div class="content">
            <div class="detail-btn">
                <a href="">
                    <img :src="index==0?left.img:right.img" alt="" class="preview fl">
                    <span class="dish-name fl">{{index==0?left.dish_name:right.dish_name}}</span>
                    <span class="look-over fr">查看学做菜谱 ></span>
                </a>
            </div>
            <ul class="list" ref="list">
                <li class="uploads-btn fl">
                    +上传你的作品
                </li>
                <li class="list-item" v-for="item in list">
                    <a href=""><img :src="item.img" alt=""></a>
                    <div class="user-info">
                        <img :src="item.portrait" alt="" class="fl">
                        <dl class="fl">
                            <dd class="user-name">{{item.user_name | substr(0,6)}}</dd>
                            <dd class="post-date">{{item.post_date}}</dd>
                        </dl>
                    </div>
                    <div class="dish-des">{{item.des}}</div>
                    <div class="like-btn" :class="item.like?'like':'dislike'"></div>
                    <p class="like-num">{{item.like_num}}</p>
                </li>
            </ul>
        </div>
    </div>
</template>
<style type="text/css">
    *{margin:0;padding:0;list-style: none}
    .fl{float:left;}
    .fr{float: right;}
    .faces{overflow: hidden;padding:0.417rem 0.417rem 0;}
    .faces a{display: inline-block;width:7.29rem;height:8.75rem;}
    .faces a img{max-width: 100%;}
    .recipe-pk-detail{padding:1.458rem 0 1.25rem;}
    .recipe-pk-detail ul{padding:0 1.0417rem 1.458rem;background: url("/images/pk.png") no-repeat center top;background-size: 2.083rem 2.083rem;overflow: hidden;}
    .recipe-pk-detail ul>li{width:49.9%;}
    .rate{font-size: 0.833rem;font-weight: bolder;margin-bottom: 0.417rem;}
    .left-rate{color: #ff6e6e;}
    .right-rate{color:#46c4ff;text-align: right;}
    .recipe-num{color: #b0b0b0;font-size: 0.5rem;}
    .right-align{text-align: right}
    .pk-desc{font-size: 0.583rem;padding: 0 0.625rem;}
    .pk-tab-wrap{overflow: hidden;border-bottom: 1px solid #e6e2df;border-top: 1px solid #e6e2df;}
    .pk-tab-wrap li{height: 2.083rem;width:49.9%;text-align: center;line-height: 2.083rem;}
    .pk-tab-wrap li span{color:#333333;font-weight: bold;font-size: 0.625rem;padding:0 0.417rem 0;position: relative;display: inline-block;}
    .pk-tab-wrap li span.active:after{position:absolute;height:0.1833rem;left:0;bottom: 0;background: #000;content: ' ';width:100%;}
    .detail-btn{height: 1.67rem;line-height: 1.67rem;padding:0.417rem;}
    .preview{height:1.667rem;width: 1.667rem;}
    .detail-btn a{color:#333333;}
    .dish-name{font-size:0.583rem;margin-left: 0.417rem;}
    .look-over{font-size: 0.5rem;color:#ff8c5f;margin-right:0.625rem;}
    .list{overflow: hidden;padding:0 0.417rem;}
    .list li{width:7.29rem;}
    .list-item{width:7.29rem;}
    .list-item img{max-width:100%;}
    .list-item:nth-of-type(odd){float: left}
    .list-item:nth-of-type(even){float: right}
    .user-info{overflow: hidden;padding:0.625rem 0.417rem 0.833rem;}
    .user-info dl{margin-left: 0.417rem;}
    .user-info img{height:1.125rem;width:1.125rem;border-radius: 50%;}
    .user-name{font-size: 0.5rem;color:#333333;line-height:0.5rem;margin-bottom: 0.208rem;}
    .post-date{font-size:0.375rem;line-height: 0.375rem;color: #a7a7a7;}
    .dish-des{padding:0 0.417rem;font-size: 0.5rem;height:1rem;line-height: 0.5rem;}
    .like-btn{height:0.583rem;margin:0.833rem auto 0.292rem;}
    .like{background: url("/images/like.png") no-repeat center center;background-size: 0.583rem 0.583rem;}
    .dislike{background: url("/images/dislike.png") no-repeat center center;background-size: 0.583rem 0.583rem;}
    .like-num{text-align: center;color:#a7a7a7;font-size: 0.417rem;padding-bottom: 0.625rem;}
</style>
<script>
    var $ = require('jquery');
    module.exports = {
        data:function(){
            return {
                'left': {
                    'img': '',
                    'rate': 0,
                    'dish_num': 0,
                    'id':0,
                    'dish_name':''
                },
                'right':{
                    'img':'',
                    'rate':0,
                    'dish_num':0,
                    'id':0,
                    'dish_name':''
                },
                'des':'',
                'index':0,
                'list':[]
            };
        },
        methods:{
            toggle:function(){
                this.index = this.index==0?1:0;
                this.getInfo();
            },
            getInfo:function(){
                var that = this;
                $.ajax({
                    'url':'/list.php',
                    'async':false,
                    'type':'POST',
                    'data':'index='+that.index,
                    'success':function(data){
                        var data = $.parseJSON(data);
                        that.list = data;
                    }
                });
            }
        },
        beforeMount:function(){
            var that = this;
            console.log('beforeUpdate');
            $.ajax({
                'url':'/pk.php',
                'async':false,
                'type':'POST',
                'success':function(data){
                    var data = $.parseJSON(data);
                    console.log(data)
                    that.left = data.left;
                    that.right = data.right;
                    that.des = data.des;
                    that.index = data.index;
                    that.getInfo();
                }
            });
        },
        mounted:function () {
            console.log(this.$refs.list.children);
            if(this.list.length){
                var children = this.$refs.list.children;
                console.log(children[0].offsetHeight);
                for(var i=0;i<children.length;i++){
                    console.log(11);
                    children[i].style.height = 15+'rem';
                }
            }
        },
        filters:{
            substr:function(s,from,to){
                return s.substr(from,to);
            }
        }
    }
</script>

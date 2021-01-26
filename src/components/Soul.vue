<template>
    <body>
    <div id="outer">
        <el-header style="width: 450px;position: absolute;left: 500px" ref="card1" >
            <el-card style="height: 200px;background-color: lemonchiffon" class="login-container" >
                <el-row style="text-align: left">图片对选择</el-row>
                <el-carousel type="card" height="400px" style="margin-left: 20px" @change="onChange" :autoplay="false" >
                    <el-carousel-item v-for="item in imagebox" :key="item.id">
                        <img :src="item.idView" class="image" width="150px" height="150px">
                    </el-carousel-item>
                </el-carousel>
            </el-card>
        </el-header>
        <div class="box" ref="box">
            <div class="small" ref="small" @mouseenter="onmouseenter" @mouseleave="onmouseleave" @mousemove="onmousemove">
                <img :src="imgarr1[index]" alt="" />
                <div class="mask" ref="mask"></div>
                <div class="mask2" ref="mask2"></div>
            </div>
            <div class="big" ref="big">
                <img :src="imgarr3[index]" alt="" ref="bigImg" style="width: 1024px;height: 1024px"/>
<!--                <img src="@/big1/1.png" >-->
            </div>
            <div class = "small2" ref="small2">
                <img :src = "imgarr2[index]" alt=""/>
            </div>
            <div class="big2" ref="big2">
                <img :src="imgarr4[index]" alt="" ref="bigImg2" style="width: 1024px;height: 1024px"/>
            </div>
        </div>
    </div>
        <!-- <button id="pre">上一张</button>
        <button id="next">下一张</button> -->
    </body>
</template>

<script>
    export default {
        name: "Soul",
        data(){
            return {
                visable:true,
                imgarr1: [require("../img1/1.png"), require("@/img1/2.png"), require("@/img1/3.png")],
                imgarr2: [require("../img2/4.png"), require("@/img2/5.png"), require("@/img2/6.png")],
                imgarr3: [require("../big1/1.png"), require("@/big1/2.png"), require("@/big1/3.png")],
                imgarr4: [require("../big2/4.png"), require("@/big2/5.png"), require("@/big2/6.png")],
                index: 0,
                imagebox:[{id:0,idView:require('../img1/1.png')},
                    {id:1,idView:require('../img1/2.png')},
                    {id:2,idView:require('../img1/3.png')},
                ]
            }
        },
        methods:{
            //1.鼠标放上去显示盒子，移开隐藏盒子。(为小盒子绑定事件)
            onmouseenter ()
                {
                //封装好方法调用：显示元素
                    this.show(this.$refs.mask);
                    this.show(this.$refs.mask2);
                    this.show(this.$refs.big2);
                    this.show(this.$refs.big);
                    this.hide(this.$refs.card1)
            },
            onmouseleave ()
                {
                //封装好方法调用：隐藏元素
                this.hide(this.$refs.mask);
                this.hide(this.$refs.mask2);
                this.hide(this.$refs.big2);
                this.hide(this.$refs.big);
                this.show(this.$refs.card1)
            },

            onChange(pre_id,now_id){
                this.index=pre_id
            },

            //2.老三步和新五步（黄盒子跟随移动）
            //绑定的事件是onmousemove，而事件源是this.small(只要在小盒子上移动1像素，黄盒子也要跟随)
            onmousemove (event)
                {
                //新五步
                event = event || window.event;

                //想要移动黄盒子，必须要知道鼠标在this.small小图中的位置。
                var pagex = event.pageX || this.scroll().left + event.clientX;
                var pagey = event.pageY || this.scroll().top + event.clientY;
                var pagey2 = event.pageY + 350|| this.scroll().top + event.clientY + 512;

                //x：mask的left值，y：mask的top值。
                var x = pagex - this.$refs.box.offsetLeft - this.$refs.mask.offsetWidth / 2; //除以2，可以保证鼠标mask的中间
                var y = pagey - this.$refs.box.offsetTop - this.$refs.mask.offsetHeight / 2;
                var x2 = pagex - this.$refs.box.offsetLeft - this.$refs.mask2.offsetWidth / 2; //除以2，可以保证鼠标mask的中间
                var y2 = pagey2 - this.$refs.box.offsetTop - this.$refs.mask2.offsetHeight / 2 ;
                //限制换盒子的范围
                //left取值为大于0，小盒子的宽-mask的宽。
                if (x < 0) {
                    x = 0;
                }
                if (x2 < 0) {
                    x2 = 0;
                }
                if (x > this.$refs.small.offsetWidth - this.$refs.mask.offsetWidth) {
                    x = this.$refs.small.offsetWidth - this.$refs.mask.offsetWidth;
                }
                if (x2 > this.$refs.small2.offsetWidth - this.$refs.mask2.offsetWidth) {
                    x2 = this.$refs.small2.offsetWidth - this.$refs.mask2.offsetWidth;
                }
                //top同理。
                if (y < 0) {
                    y = 0;
                }
                if (y2 < 350) {
                    y2 = 350;
                }
                if (y > this.$refs.small.offsetHeight - this.$refs.mask.offsetHeight) {
                    y = this.$refs.small.offsetHeight - this.$refs.mask.offsetHeight;
                }




                //移动黄盒子
                this.$refs.mask.style.left = x + "px";
                this.$refs.mask.style.top = y + "px";
                this.$refs.mask2.style.left = x2 + "px";
                this.$refs.mask2.style.top = y2  + "px";

                //3.右侧的大图片，等比例移动。
                //如何移动大图片？等比例移动。
                //    大图片/大盒子 = 小图片/mask盒子
                //    大图片走的距离/mask走的距离 = （大图片-大盒子）/（小图片-黄盒子）
//                var bili = (bigImg.offsetWidth-big.offsetWidth)/(this.small.offsetWidth-mask.offsetWidth);

                //大图片走的距离/mask盒子都的距离 = 大图片/小图片
                var bili = this.$refs.bigImg.offsetWidth / this.$refs.small.offsetWidth;
                var xx = bili * x;  //知道比例，就可以移动大图片了
                var yy = bili * y;
                var bili2 = this.$refs.bigImg2.offsetWidth / this.$refs.small2.offsetWidth;
                var xx2 = bili2 * x;  //知道比例，就可以移动大图片了
                var yy2 = bili2 * y;
                this.$refs.bigImg.style.marginTop = -yy + "px";
                this.$refs.bigImg.style.marginLeft = -xx + "px";
                this.$refs.bigImg2.style.marginTop = -yy2 + "px";
                this.$refs.bigImg2.style.marginLeft = -xx2 + "px";
            }


            ,hide(ele) {
                ele.style.display = "none";
            },show(ele) {
                ele.style.display = "block";
            },scroll() {  // 开始封装自己的scrollTop
                if (window.pageYOffset != null) {  // ie9+ 高版本浏览器
                    // 因为 window.pageYOffset 默认的是  0  所以这里需要判断
                    return {
                        left: window.pageXOffset,
                        top: window.pageYOffset
                    }
                }
                else if (document.compatMode === "CSS1Compat") {    // 标准浏览器   来判断有没有声明DTD
                    return {
                        left: document.documentElement.scrollLeft,
                        top: document.documentElement.scrollTop
                    }
                }
                return {   // 未声明 DTD
                    left: document.body.scrollLeft,
                    top: document.body.scrollTop
                }
            }

        }
    }
</script>

<style scoped>
    #outer{
        background: url("../assets/1.jpg") no-repeat center;
        height: 100%;
        width: 80%;
        background-size: contain;
        position: fixed;
    }

    .box {
        width: 350px;
        height: 350px;
        margin: 0px;
        border: 1px solid #ccc;
        position: relative;
    }

    .big {
        width: 400px;
        height: 400px;
        position: absolute;
        top: 240px;
        left: 480px;
        /*border: 1px solid #ccc;*/
        overflow: hidden;
        display: none;
        border-radius: 15px;
    }
    .big2 {
        width: 400px;
        height: 400px;
        position: absolute;
        top: 240px;
        left: 970px;
        /*border: 1px solid #ccc;*/
        overflow: hidden;
        display: none;
        border-radius: 15px;
    }

    .mask {
        width: 100px;
        height: 100px;
        background: rgba(0, 0, 0, 0.5);
        position: absolute;
        top: 0;
        left: 0;
        cursor: move;
        display: none;
    }
    .mask2 {
        width: 100px;
        height: 100px;
        background: rgba(0, 0, 0, 0.5);
        position: absolute;
        top: 0;
        left: 0;
        cursor: move;
        display: none;
        z-index: 1000000;
    }

    .small {
        position: relative;
    }
    .small2{
        position: relative;
        z-index: 20;
    }

    .img {
        vertical-align: top;
    }
    .ptype{
        font-weight:bold;
        font-size:25px;
        text-shadow: 5px 5px 5px #558081;
        font-family: SimHei;
    }

    .el-carousel__item {
        width: 150px;
        height: 150px;
        color: #475669;
        font-size: 14px;
        opacity: 1;
        line-height: 100px;
        margin: 0;
    }

    .el-carousel__item:nth-child(2n) {
        background-color: #99a9bf;
    }

    .el-carousel__item:nth-child(2n+1) {
        background-color: #d3dce6;
    }
    .login-container{
        border-radius: 15px;
        background-clip: padding-box;
        opacity: 90%;
    }
</style>
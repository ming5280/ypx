<template>
   <div id="userCenter">
       <!--导航栏-->
       <van-nav-bar
               title="用户中心"
               left-arrow
               :fixed=true
               :border=true
               @click-left="$router.go(-1)"
       ></van-nav-bar>
       <van-cell-group style="margin-top: 2.9rem">
            <van-cell 
                is-link 
                center
                
            >
                <template slot="title">
                    <span>用户头像</span>
                </template>  
                <img :src="base64" alt="" style="height:50px;width:50px;border-radius:50%;margin-top:0.3rem">
                <input type="file" name="file" class="upload-img" accept="image/*" @change="uploadImg()" ref="inputer"/>
            </van-cell>
            <van-cell 
                is-link 
                center
                @click="selectSex()"
            >
            
                <template slot="title">
                    <span>性别</span>
                </template>
                <span>{{sex || '请选择'}}</span>
            </van-cell>
            <van-cell 
                is-link 
                center
                @click="selectDate()"
            >
                <template slot="title">
                    <span>生日</span>
                </template>
                <span>{{date || '请选择'}}</span>
            </van-cell>
            <van-cell 
                is-link 
                center
               
            >
                <template slot="title">
                    <span>手机号</span>
                </template>
                <span>12345678912</span>
            </van-cell>
            <van-cell 
                is-link 
                center
                @click="selectHeight()"
            >
                <template slot="title">
                    <span>身高</span>
                </template>
                <span>{{height || '请选择'}}</span>
            </van-cell>
            <van-cell 
                is-link 
                center
                @click="selectWeight()"
            >
                <template slot="title">
                    <span>体重</span>
                </template>
                <span>{{weight || '请选择'}}</span>
            </van-cell>
        </van-cell-group>
        <div class="edit">
           <button @click="edit">确认修改</button>
       </div>
       <div class="logout">
           <button @click="logOut">退出登录</button>
       </div>
       <!-- 生日选择 -->
       <van-popup
            v-model="showDate"
            position="bottom"
            :style="{ height: '40%' }"
            >
                <van-datetime-picker
                v-model="currentDate"
                type="date"
                :min-date="minDate"
                :max-date="maxDate"
                @confirm="defineDate"
                @cancel="closeDate"
                />
        </van-popup>
        <!-- 性别选择 -->
        <van-popup
            v-model="showSex"
            position="bottom"
            :style="{ height: '35%' }"
            >
                <van-picker 
                    :columns="columnsSex" 
                    @change="onChangeSex" 
                    :show-toolbar="true"
                    @confirm="defineSex"
                    @cancel="closeSex"
                    />  
        </van-popup>
        <!-- 身高选择 -->
        <van-popup
            v-model="showHeight"
            position="bottom"
            :style="{ height: '35%' }"
            >
                <van-picker 
                    :columns="columnsHeight" 
                    @change="onChangeHeight" 
                    :show-toolbar="true"
                    @confirm="defineHeight"
                    @cancel="closeHeight"
                    :default-index="31"
                    />  
        </van-popup>
        <!-- 身高选择 -->
        <van-popup
            v-model="showWeight"
            position="bottom"
            :style="{ height: '35%' }"
            >
                <van-picker 
                    :columns="columnsWeight" 
                    @change="onChangeWeight" 
                    :show-toolbar="true"
                    @confirm="defineWeight"
                    @cancel="closeWeight"
                    :default-index="15"
                    />  
        </van-popup>
       
   </div>
</template>

<script>
    import Vue from 'vue'
    import {getLogOut} from './../../../service/api/index'
    import {mapMutations} from 'vuex'
    import {Toast , Picker } from 'vant'
    Vue.use(Picker)
    export default {
        name: "UserCenter",
        data(){
            return{
                //图像
                base64:null,
                //birthday
                showDate:false, //选择生日弹窗
                currentDate: new Date(), //选中日期
                minDate: new Date(1900, 10, 1),
                maxDate: new Date(2020, 10, 1),
                date:'', //生日

                //sex
                showSex:false,
                columnsSex: ['男', '女'],
                currentSex:'',
                sex:'',

                //height
                showHeight:false,
                columnsHeight: [],
                currentHeight:'',
                height:'',

                //weight
                showWeight:false,
                columnsWeight: [],
                currentWeight:'',
                weight:'',

            }
        },
        mounted(){
            this.columnsHeight = Array(71).fill(true).map((x,i)=>`${i+130}cm`);
            this.columnsWeight = Array(116).fill(true).map((x,i)=>`${i+35}kg`);
        },
        methods: {
            ...mapMutations(["RESET_USER_INFO", "CLEAR_CART"]),
            async logOut(){
                let result = await getLogOut();
                if(result.success_code === 200){
                    // 清空本地的数据
                    this.RESET_USER_INFO();
                    // 清空本地的购物车
                    this.CLEAR_CART();
                    Toast({
                       message: '退出登录成功！',
                       duration: 500
                    });
                }
            },

            edit(){

            },
            //------------上传用户头像--------------
            
            uploadImg() {
                //var formData = new FormData();
                //formData.append('file', e.target.files[0]);
                //console.log(e.target.files[0])
                let that = this
                let inputDOM = this.$refs.inputer;
                // 通过DOM取文件数据
                console.log(inputDOM.files[0]);
                let reader = new FileReader();
                reader.readAsDataURL(inputDOM.files[0])
                reader.onload=function(e){
                    // 读取结果
                    console.log(this.result)
                    that.base64 = this.result
                    // if (size > ...) {
                    //     // 这里可以加个文件大小控制
                    //     return false
                    // }
                }
                // this.file    = inputDOM.files[0];
                // this.errText = '';

                // let size = Math.floor(this.file.size / 1024);
                // console.log(this.file)
                // //new一个FileReader实例
                // let reader = new FileReader();
                // let _this = this;
                // // reader.readAsText(this.file)  //文本读取，默认utf-8，格式 其他格式：reader.readAsText(this.file, "GBK")  
                // reader.readAsDataURL(this.file)  //base 64 读取
                // reader.onload=function(e){
                //     // 读取结果
                //     console.log(this.result)
                //     // if (size > ...) {
                //     //     // 这里可以加个文件大小控制
                //     //     return false
                //     // }
                // }


                //图片压缩上传，大于1.2M压缩图片
                // if (file.size / 1024 > 1024 * 1.2) {
                //     dealImage(image_base64, {
                //         quality: 0.5
                //     }, function(base64Codes) {
                //         var bl = processData(base64Codes, file.type);
                //         uploadPic(bl, imgId);
                //     });
                // } else {
                //     uploadPic(file, imgId);
                // }
            },
            //------------选择生日-----------------
            selectDate(){
                this.showDate = true
            },
            fomateDate(value){
                let year=value.getFullYear().toString()
                let month=(value.getMonth()+1).toString().padStart(2,'0')
                let day=value.getDate().toString().padStart(2,'0')
                return `${year}-${month}-${day}`
            },
            defineDate(){
                this.date = this.fomateDate(this.currentDate)
                this.showDate = false
            },
            closeDate(){
                this.showDate = false
            },
            //-------------选择性别-----------------
            selectSex(){
                this.showSex = true
            },
            onChangeSex(picker, value, index){
                this.currentSex = value
            },
            defineSex(){
                this.sex = this.currentSex
                this.showSex = false
            },
            closeSex(){
                this.showSex = false
            },
            //---------------身高选择--------------
            selectHeight(){
                this.showHeight = true
            },
            onChangeHeight(picker, value, index){
                this.currentHeight = value
            },
            defineHeight(){
                this.height = this.currentHeight
                this.showHeight = false
            },
            closeHeight(){
                this.showHeight = false
            },
            //---------------身高选择--------------
            selectWeight(){
                this.showWeight = true
            },
            onChangeWeight(picker, value, index){
                this.currentWeight = value
            },
            defineWeight(){
                this.weight = this.currentWeight
                this.showWeight = false
            },
            closeWeight(){
                this.showWeight = false
            }
        }
    }
</script>

<style scoped>
   #userCenter{
       position: fixed;
       top: 0;
       left: 0;
       bottom: 0;
       right: 0;
       background-color: #f5f5f5;
       z-index: 200;
   }
   .edit{
       margin-top: 1rem;
       height: 2.5rem;
       width: 100%;
       background-color: #fff;
       display: flex;
       justify-content: center;
       align-items: center;
   }
   .logout{
       margin-top: 0.3rem;
       height: 2.5rem;
       width: 100%;
       background-color: #fff;
       display: flex;
       justify-content: center;
       align-items: center;
   }

   .logout button,.edit button{
       background-color: transparent;
   }
   .upload-img{
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        opacity: 0;
   }
</style>
<template>
     <!-- 头部 -->
        <header class="header">
            <!-- 头部的第一行 -->
            <div class="top">
                <div class="container">
                    <div class="loginList" >
                        <p>尚品汇欢迎您！</p>
                        <p v-if="show">
                            <span>请</span>
                            <router-link  to="/login">登录</router-link>
                            <router-link  class="register" to="/register">免费注册</router-link>
                        </p>
                        <p v-else> 
                            <span>{{UserInfo}}</span>
                            <a class="register" href="javascript:;" @click="loginOut">退出登录</a>
                        </p>
                    </div>
                    <div class="typeList">
                        <router-link to="/center">我的订单</router-link>
                        <router-link :to="{name:'ShopCart'}" >我的购物车</router-link>
                        <a href="###">我的尚品汇</a>
                        <a href="###">尚品汇会员</a>
                        <a href="###">企业采购</a>
                        <a href="###">关注尚品汇</a>
                        <a href="###">合作招商</a>
                        <a href="###">商家后台</a>
                    </div>
                </div>
            </div>
            <!--头部第二行 搜索区域-->
            <div class="bottom">
                <h1 class="logoArea">
                    <router-link  to="/home" class="logo">
                        <img src="./images/logo.png" alt="">
                    </router-link>
                </h1>
                <div class="searchArea">
                    <form action="###" class="searchForm">
                        <input type="text" id="autocomplete" class="input-error input-xxlarge" v-model="keyword"/>
                        <button class="sui-btn btn-xlarge btn-danger" type="button" @click="goSearch">搜索</button>
                    </form>
                </div>
            </div>
        </header>
</template>

<script>
import {mapState} from 'vuex'
export default {
    name:'Header',
    data() {
        return {
            keyword:'',
            show:1,
        }
    },
    computed: {
        UserInfo() {
            return this.$store.state.user.getUserInfo.name
        },
        token: {
            get() {
                return this.$store.state.user.token
            },
            set(newV) {
                this.$store.state.user.token = newV
            }
        },
    },
    watch: {
        token(newV,oldV) {
            console.log('token变了');
            this.clearUndefined()
        }
    },
    methods: {
        goSearch() {
            // this.$router.push('/search')
            let location = { 
                name:'Search',
                params: {
                     keyword: this.keyword.toUpperCase() 
                     }
                 }
            if(this.$route.query) {
                location.query = this.$route.query
            }
            this.$router.push(location)
        },
         clearUndefined() {
         let timer = setInterval(() => {
            console.log('定时器');
            if(this.token != '') {
              this.show = 0
              clearInterval(timer)
            } else {
                this.show = 1
                clearInterval(timer)
            }
        }, 600);
        },
        // 退出登录接口
        async loginOut() {
            let result = await this.$API.reGetLoginOut()
            console.log('退出登录信息！');
            console.log(result);
            if(result.code == 200) {
                this.token = ''
                let Path = this.$route.path
                if(Path == '/center' || '/pay' || 'shopcart' || '/pauSuccess' || '/trade') {
                    this.$router.push('/home')
                }
            } else {
                alert('退出登录失败了')
            }
        }
    },
    mounted() {
        this.$bus.$on('clear', ()=> {
            this.keyword = ''
        })
    }
}
</script>

<style lang="less" scoped>
 .header {
        &>.top {
            background-color: #eaeaea;
            height: 30px;
            line-height: 30px;

            .container {
                width: 1200px;
                margin: 0 auto;
                overflow: hidden;

                .loginList {
                    float: left;

                    p {
                        float: left;
                        margin-right: 10px;

                        .register {
                            border-left: 1px solid #b3aeae;
                            padding: 0 5px;
                            margin-left: 5px;
                        }
                    }
                }

                .typeList {
                    float: right;

                    a {
                        padding: 0 10px;

                        &+a {
                            border-left: 1px solid #b3aeae;
                        }
                    }

                }

            }
        }

        &>.bottom {
            width: 1200px;
            margin: 0 auto;
            overflow: hidden;

            .logoArea {
                float: left;

                .logo {
                    img {
                        width: 175px;
                        margin: 25px 45px;
                    }
                }
            }

            .searchArea {
                float: right;
                margin-top: 35px;

                .searchForm {
                    overflow: hidden;

                    input {
                        box-sizing: border-box;
                        width: 490px;
                        height: 32px;
                        padding: 0px 4px;
                        border: 2px solid #ea4a36;
                        float: left;

                        &:focus {
                            outline: none;
                        }
                    }

                    button {
                        height: 32px;
                        width: 68px;
                        background-color: #ea4a36;
                        border: none;
                        color: #fff;
                        float: left;
                        cursor: pointer;

                        &:focus {
                            outline: none;
                        }
                    }
                }
            }
        }
    }

</style>
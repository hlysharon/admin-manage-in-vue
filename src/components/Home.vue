<template>
    <el-container class="home-container">
        <el-header>
            <div class="home-header">
                <img src="../assets/heima.png" alt="">
                <span>后台管理页面</span>
            </div>
            <el-button type="primary" @click="logout">退出</el-button>
        </el-header>
        <el-container>
            <el-aside width="200px">
                <el-menu default-active="2" class="el-menu-vertical-demo"
                background-color="#333744" text-color="#fff" active-text-color="#ffd04b">
                    <el-submenu :index="item.id + ''" v-for="item in menusList" :key="item.id">
                        <template slot="title">
                            <i :class="iconsObj[item.id]"></i>
                            <span>{{item.authName}}</span>
                        </template>
                            <el-menu-item :index="subItem.id + ''" v-for="subItem in item.children" :key="subItem.id">
                                <template slot="title">
                                    <i class="el-icon-location"></i>
                                    <span>{{subItem.authName}}</span>
                                </template>
                            </el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <el-main>Main</el-main>
        </el-container>
    </el-container>
</template>
<script>
export default {
    created() {
        this.getMenus()
    },
    data () {
        return {
            menusList: [],
            iconsObj:{
                '125':'iconfont icon-user',
                '103':'iconfont icon-tijikongjian',
                '101':'iconfont icon-shangpin',
                '102':'iconfont icon-danju',
                '145':'iconfont icon-baobiao'
            }
        };
    },
    methods:{
        logout() {
            // window.sessionStorage.clear()
            window.sessionStorage.removeItem('token')
            this.$router.push('/login')
        },
        async getMenus() {
            const {data:res} = await this.$http.get('menus');
            if(res.meta.status !== 200) return this.$message.error('请求失败')
            this.menusList = res.data
        }
    }
}
</script>
<style scoped lang="less">
    .home-container {
        height: 100%;
        .el-header {
            padding-left: 0;
            background-color: #373d41;
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: #fff;
            .home-header {
                display: flex;
                align-items: center;
                span {
                    font-weight: 700;
                    margin-left: 10px;
                }
            }
        }
        .el-aside {
            background-color: #333744;
            .el-menu {
                border-right: none;
            }
        }
        .el-main {
            background-color: #eaedf1;
        }
    }
</style>
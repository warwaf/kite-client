<template>
  <div class="main-header">
    <header class="navbar navbar-visible">
      <div class="container navbar-view">
        <router-link :to="{ name: 'home' }"
                     class="navbar-brand logo-img"
                     v-if="website.meta.logo"
                     :style="{ 'background-image': 'url(' + website.meta.logo + ')' }"></router-link>
        <router-link :to="{ name: 'home' }"
                     class="navbar-brand logo-text"
                     v-else>{{ website.meta.website_name }}</router-link>
        <div class="collapse navbar-collapse">
          <ul class="navbar-item-content mr-auto">
            <li class="navbar-menu-content active">
              <div class="navbar-toggler"
                   @click="isNavbarMenu = !isNavbarMenu">
                <Dropdown>
                  <div class="el-dropdown-link"
                       slot="button">
                    <i class="menu-icon el-icon-menu"></i>
                  </div>
                  <div class="dropdown-menu-view">
                    <router-link :to="{ name: 'home' }"
                                 class="dropdown-menu-item">首页</router-link>
                    <router-link :to="{
                        name: 'dynamics',
                        params: { dynamicTopicId: 'newest' }
                      }"
                                 class="dropdown-menu-item">片刻</router-link>
                    <router-link :to="{ name: 'books', params: { columnEnName: 'all' } }"
                                 class="dropdown-menu-item">小书</router-link>
                    <router-link v-if="personalInfo.islogin"
                                 class="dropdown-menu-item"
                                 :to="{
                        name: 'AttentionMessage'
                      }">
                      关注
                      <span v-if="user.attentionCount > 0"
                            class="unread-message-count">{{ user.attentionCount }}</span>
                    </router-link>
                    <router-link v-if="personalInfo.islogin"
                                 class="dropdown-menu-item"
                                 :to="{name: 'user',params: {
                          uid: personalInfo.user.uid,
                          routeType: 'message'
                        }
                      }">
                      消息
                      <span v-if="user.messageCount > 0"
                            class="unread-message-count">{{ user.messageCount }}</span>
                    </router-link>
                    <router-link v-if="personalInfo.islogin"
                                 class="dropdown-menu-item"
                                 :to="{name: 'privateChatList'}">
                      私聊
                      <span v-if="user.messageCount > 0"
                            class="unread-message-count">{{ user.privateChatCount }}</span>
                    </router-link>
                  </div>
                </Dropdown>
              </div>
              <ul class="navbar-menu"
                  :class="{ show: isNavbarMenu }">
                <li class="nav-item"
                    :class="{'active':$route.name==='home'}">
                  <router-link :to="{ name: 'home' }"
                               class="nav-link">首页</router-link>
                </li>
                <li class="nav-item"
                    :class="{'active':$route.name==='dynamics'}">
                  <router-link :to="{
                      name: 'dynamics',
                      params: { dynamicTopicId: 'newest' }
                    }"
                               class="nav-link">片刻</router-link>
                </li>
                <li class="nav-item"
                    :class="{'active':$route.name==='books'}">
                  <router-link :to="{ name: 'books', params: { columnEnName: 'all' } }"
                               class="nav-link">小书</router-link>
                </li>
              </ul>
            </li>
            <li class="nav-item search">
              <div class="form-search form-inline mr-lg-5">
                <input class="form-control form-search-view"
                       type="text"
                       required="true"
                       v-model="searchVal"
                       name="search"
                       placeholder="搜索文章"
                       aria-label="Search" />
                <button class="search-btn"
                        @click="search">
                  <i class="el-icon-search"></i>
                </button>
              </div>
            </li>
            <template v-if="personalInfo.islogin">
              <li class="nav-item icon-item">
                <router-link :to="{ name: 'Write', params: { type: 'create' } }"
                             class="btn btn-sm ">
                  <i class="iconfont el-icon-edit"></i>
                </router-link>
              </li>
              <li class="nav-item icon-item mb-hide">
                <router-link class="btn btn-sm no-read-msg"
                             :to="{
                    name: 'AttentionMessage'
                  }">
                  <i class="iconfont el-icon-timer"></i>
                  <span v-if="user.attentionCount > 0"
                        class="unread-message-count">{{ user.attentionCount }}</span>
                </router-link>
              </li>
              <li class="nav-item  icon-item mb-hide">
                <router-link class="btn btn-sm no-read-msg"
                             :to="{
                    name: 'user',
                    params: { uid: personalInfo.user.uid, routeType: 'message' }
                  }">
                  <i class="iconfont el-icon-bell"></i>
                  <span v-if="user.messageCount > 0"
                        class="unread-message-count">{{ user.messageCount }}</span>
                </router-link>
              </li>
              <li class="nav-item   icon-item mb-hide">
                <router-link class="btn btn-sm no-read-msg"
                             :to="{
                    name: 'privateChatList'
                  }">
                  <i class="iconfont el-icon-chat-line-round"></i>
                  <span v-if="user.privateChatCount > 0"
                        class="unread-message-count">{{ user.privateChatCount }}</span>
                </router-link>
              </li>
              <li class="nav-item dropdown user-center">
                <Dropdown placement="right">
                  <div class="el-dropdown-link"
                       slot="button">
                    <div class="avatar-img">
                      <img v-lazy="personalInfo.user.avatar"
                           class="box-image"
                           alt="" />
                    </div>
                  </div>
                  <div class="dropdown-menu-view">
                    <router-link class="dropdown-menu-item"
                                 :to="{
                        name: 'user',
                        params: {
                          uid: personalInfo.user.uid,
                          routeType: 'article'
                        }
                      }">
                      <i class="item-icon el-icon-user"></i>
                      我的主页</router-link>
                    <router-link class="dropdown-menu-item"
                                 :to="{ name: 'myOrder' }">
                      <i class="item-icon el-icon-notebook-1"></i>
                      我的订单</router-link>
                    <router-link class="dropdown-menu-item"
                                 :to="{ name: 'experienceDetail' }">
                      <i class="item-icon el-icon-document-checked"></i>
                      经验明细</router-link>
                    <router-link class="dropdown-menu-item"
                                 :to="{ name: 'shellDetail' }">
                      <i class="item-icon el-icon-notebook-2"></i>
                      贝壳明细</router-link>
                    <router-link class="dropdown-menu-item"
                                 :to="{ name: 'collect' }">
                      <i class="item-icon el-icon-folder-opened"></i>
                      我的收藏</router-link>
                    <router-link class="dropdown-menu-item"
                                 :to="{ name: 'setting' }">
                      <i class="item-icon el-icon-setting"></i>
                      设置</router-link>
                    <div class="dropdown-menu-item"
                         @click="escLogin">
                      <i class="item-icon el-icon-right"></i>
                      退出
                    </div>
                  </div>
                </Dropdown>
              </li>
            </template>
            <template v-else>
              <li class="nav-item"
                  v-if="website.config.on_login === 'yes'">
                <router-link class="btn btn-sm sign-btn btn-block"
                             :to="{ name: 'signIn' }">登录</router-link>
              </li>
              <li class="nav-item"
                  v-if="website.config.on_register === 'yes'">
                <router-link class="btn s-btn--primary btn-sm sign-btn btn-outline-warning"
                             :to="{ name: 'signUp' }">注册</router-link>
              </li>
            </template>
          </ul>
        </div>
      </div>
    </header>
  </div>
  <!--header end-->
</template>

<script>
import { cookie } from '../../utils/cookie.js'
import { mapState } from 'vuex'
import { Dropdown } from '@components'
export default {
  name: 'Header',
  data () {
    return {
      isNavbarMenu: false, // 主菜单栏是否显示
      isDropdownMenu: false, // 个人下拉菜单栏是否显示
      searchVal: ''
    }
  },
  methods: {
    search () {
      if (!this.searchVal) {
        this.$message.warning('请输入搜索内容')
        return false
      }
      this.$router.push({
        name: 'search',
        query: { query: this.searchVal }
      })
    },
    escLogin () {
      this.$message.warning('已退出当前账户，请重新登录')
      cookie.delete('accessToken')
      window.location.reload()
    }
  },
  components: {
    Dropdown
  },
  computed: {
    ...mapState(['website', 'personalInfo', 'user']) // home:主页  article_column:文章的专栏
  }
}
</script>

<style scoped lang="scss">
.main-header {
  position: relative;
  height: 65px;
  .header-visible {
    transform: translateZ(0);
  }

  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    transition: all 0.2s;
    transform: translate3d(0, -100%, 0);
    background: #fff;
    /* border-bottom: 1px solid #f1f1f1;*/
    color: #909090;
    z-index: 250;
    // -webkit-box-shadow: 0 1px 3px rgba(26, 26, 26, 0.1);
    // box-shadow: 0 1px 3px rgba(26, 26, 26, 0.1);
    box-shadow: 0 1px 8px rgba(0, 0, 0, 0.05);
    // box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.05);
    background-clip: content-box;
    &.navbar-visible {
      transform: translateZ(0);
    }
    .navbar-view {
      display: flex;
      align-items: center;
      height: 100%;
      .navbar-brand {
        margin-right: 30px;
        &.logo-text {
          font-size: 25px;
          color: #e67e7e;
        }
        &.logo-img {
          background-size: 100% 100%;
          display: block;
          width: 90px;
          height: 32px;
          /* position: absolute;*/
          left: 10%;
        }
      }
      .navbar-collapse {
        height: 100%;
        flex: 1 0 auto;
        .navbar-item-content {
          display: flex;
          align-items: center;
          justify-content: flex-end;
          position: relative;
          height: 100%;
          margin: 0;
          .navbar-menu-content {
            display: flex;
            flex: 1;
            align-items: center;
            .navbar-menu {
              display: flex;
              justify-content: flex-start;
            }
          }
          .navbar-toggler {
            display: none;
            .menu-icon {
              width: 50px;
              height: 50px;
              text-align: center;
              line-height: 50px;
              color: #999;
              font-size: 18px;
            }
          }
          .nav-item {
            position: relative;
            font-size: 18px;
            margin: 0;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            > a {
              padding: 15px 15px;
              display: block;
              font-size: 15px;
              color: #252a2d;
            }
            .s-btn--primary {
              color: #fff;
            }
            i {
              display: inline-block;
            }
            .iconfont {
              font-size: 20px;
            }
            &.active {
              a {
                color: #ea6f5a;
              }
            }

            &.icon-item {
              i {
                color: #333;
              }
            }
            &.search {
              margin-right: 30px;
              .form-search {
                border: 1px solid hsla(0, 0%, 59.2%, 0.2);
                background-color: rgba(227, 231, 236, 0.2);
                display: flex;
                align-items: center;
                justify-content: space-between;
                border-radius: 6px;
                .form-search-view {
                  border: none;
                  padding: 6px 10px;
                  box-shadow: none;
                  outline: none;
                  font-size: 12px;
                  color: #666;
                  width: 200px;
                  background-color: transparent;
                }
                .search-btn {
                  border: none;
                  padding: 0 8px;
                  background-color: transparent;
                }
              }
            }
            &.user-center {
              margin-left: 20px;
            }
            &.dropdown {
              position: relative;
              /deep/ .el-dropdown {
                height: 36px;
              }
              .item-icon {
                font-size: 16px;
                display: inline-block;
                margin-right: 5px;
                vertical-align: middle;
              }
              .avatar-img {
                display: inline-block;
                position: relative;
                width: 36px;
                height: 36px;
                border-radius: 72px;
                .box-image {
                  width: 36px;
                  height: 36px;
                  border-radius: 4px;
                  overflow: hidden;
                  img {
                    width: 100%;
                    height: 100%;
                    border-radius: 80px;
                  }
                }
              }
            }
            .btn-outline-warning {
              border-radius: 20px;
              padding: 5px 20px;
            }
            .sign-btn {
              font-size: 14px;
            }
            .no-read-msg {
              position: relative;
              .unread-message-count {
                background: #ff4d4f;
                border-radius: 50%;
                position: absolute;
                display: block;
                font-size: 12px;
                width: 15px;
                text-align: center;
                line-height: 15px;
                height: 15px;
                right: 12px;
                top: 4px;
                color: #fff;
              }
            }
          }
        }
      }
    }
  }
}

@media (max-width: 575px) {
  body {
    .main-header {
      .navbar {
        .navbar-view {
          .navbar-brand {
            &.logo-img {
              position: static;
            }
          }
          .navbar-collapse {
            .navbar-item-content {
              .nav-item {
                &.search {
                  display: none;
                }
                &.mb-hide {
                  display: none;
                }
              }
              .navbar-toggler {
                display: block;
                width: 50px;
                height: 50px;
                line-height: 50px;
                text-align: center;
              }
              .navbar-menu {
                display: none !important;
              }
            }
          }
        }
      }
    }
  }
}

@media (min-width: 768px) {
}

@media (min-width: 992px) {
}

@media (min-width: 1200px) {
}
</style>

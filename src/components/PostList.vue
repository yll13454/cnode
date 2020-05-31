<template>
    <div class="postlist">
        <!-- <div class="loading" v-if="isLoading">
            <img src="@/assets/loading.gif" alt="" srcset="">
        </div> -->
        <ul class="post">
            <li>
                <ul class="toobar">
                    <li class="checkout">全部</li>
                    <li>精华</li>
                    <li>分享</li>
                    <li>问答</li>
                    <li>招聘</li>
                    <li>客户端测试</li>
                </ul>
            </li>
            <li v-for="post in posts" :key="post.id">
                <span class="postwrapper">
                    <router-link :to="{
                        name:'user_info',
                        params:{
                            name:post.author.loginname
                        }
                    }">
                        <img :src="post.author.avatar_url" alt="头像">
                    </router-link>
                    <span class="readnum">
                        <span>{{post.reply_count}}</span>/
                        <span>{{post.visit_count}}</span>
                    </span>
                    <span :class="classes(post)">{{post | tabFormatter}}</span>
                    <router-link :to="{name:'post_content',params:{id:post.id,name:post.author.loginname}}">
                        <span class="linkclass">{{post.title}}</span>
                    </router-link>
                </span>
                <span class="datesty">
                    {{post.last_reply_at | formatDate}}
                </span>
            </li>
            <li>
                <pagination @handlelist="renderlist"></pagination>
            </li>
        </ul>
    </div>
</template>

<script>
import pagination from '@/components/Pagination'
export default {
  data() {
    return {
      isLoading: false,
      posts: [],
      postpage: 1
    }
  },
  components: {
    pagination
  },
  methods: {
    getData() {
        this.isLoading = true
      this.$http
        .get('https://cnodejs.org/api/v1/topics', {
          params: {
            page: this.postpage,
            limit: 20
          }
        })
        .then(res => {
          this.posts = res.data.data
          this.isLoading = false
        })
        .catch(err => {
          console.log(err)
        })
    },
    classes(post) {
      return [
        {
          put_good: post.good === true || post.top === true,
          'topiclist-tab': post.good !== true && post.top !== true
        }
      ]
    },
    renderlist(value) {
      this.postpage = value
      this.getData()
      window.scrollTo(0, 0)
    }
  },
  beforeMount() {
    this.getData()
  }
}
</script>

<style lang="scss" scoped>
.postlist {
  font-size: 15px;
  position: relative;
  display: flex;
  justify-content: center;
  white-space: nowrap;
  flex-grow: 1;
  a {
    color: black;
  }
  .loading {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) ；;
  }
  > .post {
    width: 100%;
    li:first-child {
      background-color: rgb(245, 245, 245);
    }
    > li {
      display: flex;
      align-items: center;
      height: 50px;
      background-color: rgb(255, 255, 255);
      border-bottom: 0.5px solid #f5f5f5;
      padding: 0 0.5em;
      justify-content: space-between;
      &:hover {
        background-color: rgb(245, 245, 245);
      }
      .toobar {
        display: flex;
        align-items: center;
        color: rgb(128, 189, 1);
        li {
          margin: 0 12px;
        }
        .checkout {
          border-radius: 4px;
          padding: 3px;
          color: white;
          background-color: rgb(128, 189, 1);
        }
      }
      .readnum {
        display: inline-flex;
        justify-content: center;
        align-items: center;
        width: 60px;
        font-size: 14px;
        span:first-child {
          color: #9e78c0;
        }
        span:last-child {
          color: #bab4ba;
          font-size: 12px;
        }
      }
      .linkclass:hover {
        text-decoration: underline;
      }
      .put_good,
      .topiclist-tab {
        font-size: 12px;
        border-radius: 4px;
        padding: 2px;
      }
      .put_good {
        color: white;
        background-color: rgb(128, 189, 1);
      }
      .topiclist-tab {
        background-color: rgb(229, 229, 229);
        color: rgb(153, 153, 153);
      }
    }
    .datesty {
      font-size: 12px;
      color: rgb(153, 153, 153);
    }
    img {
      width: 30px;
      height: 30px;
      vertical-align: middle;
    }
  }
}
.loading {
  text-align: center;
  padding-top: 300px;
  z-index: 1;
}
</style>

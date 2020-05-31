<template>
    <div class="userinfo">
        <div class="preinfo">
            <p>/主页</p>
            <div class="infowrapper">
                <span class="prewrapper">
                    <img :src="posts.avatar_url" alt="" srcset=""> {{posts.loginname}}
                </span>
                <div>
                    <span>{{posts.score}}</span> 积分</div>
                <span class="creatclass">注册时间 {{posts.create_at | formatDate}}</span>
            </div>
        </div>
        <div class="lastjion">
            <p>最近创建的话题</p>
            <ul>
                <li v-for="item in posts.recent_topics" :key="item.id">
                    <span class="linkclass">
                        <router-link :to="{
                            name:'post_content',
                            params:{
                                id:item.id,
                                name:item.author.loginname
                            }
                        }">
                            {{item.title}}
                        </router-link>
                    </span>
                    <span class="timeclass">{{item.last_reply_at | formatDate}}</span>
                </li>
            </ul>
        </div>
        <div class="lastreply">
            <p>最近参与的话题</p>
            <ul>
                <li v-for="item in posts.recent_replies" :key="item.id">
                    <span class="linkclass">
                        <router-link :to="{
                            name:'post_content',
                            params:{
                                id:item.id,
                                name:item.author.loginname
                            }
                        }">
                            {{item.title}}
                        </router-link>
                    </span>
                    <span class="timeclass">{{item.last_reply_at | formatDate}}</span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      posts: []
    }
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          if (res.data.success) {
            this.posts = res.data.data
          }
        })
        .catch(err => {
          console.log(err)
        })
    }
  },
  watch: {
    '$route'(to, from) {
      this.getData()
    }
  },
  beforeMount() {
    this.getData()
  }
}
</script>

<style lang="scss" scoped>
.userinfo {
  flex-grow: 1;
  div {
    background-color: white;
  }
  .infowrapper {
    padding: 12px 12px 30px;
    font-size: 14px;
    div span {
      font-size: 13px;
    }
  }
  .preinfo {
    p {
      color: rgb(128, 189, 1);
      display: flex;
      align-items: center;
      height: 42px;
      padding: 0 12px;
      border-bottom: 1px solid rgb(255, 255, 255);
      background-color: rgb(246, 246, 246);
      margin: 0;
      margin-top: 12px;
    }
    .prewrapper {
      display: flex;
      align-items: center;
      color: rgb(191, 143, 135);
      padding-bottom: 4px;
      img {
        height: 42px;
        width: 42px;
        padding: 8px 8px 8px 0;
      }
    }
    .creatclass {
      margin-top: 12px;
      display: inline-block;
      color: rgb(189, 171, 171);
    }
  }
}
.lastjion,
.lastreply {
  p {
    height: 42px;
    display: flex;
    align-items: center;
    font-size: 14px;
    padding-left: 12px;
    margin-bottom: 0;
    background-color: rgb(246, 246, 246);
  }
  ul {
    margin: 0;
  }
  li {
    border-bottom: 1px solid rgb(246, 246, 246);
    height: 52px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 12px;
  }
  .linkclass:hover {
    text-decoration: underline;
  }
  .timeclass {
    font-size: 8px;
    color: rgb(0, 85, 128);
  }
}
</style>

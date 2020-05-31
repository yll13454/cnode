<template>
    <div class="article">
        <div class="maincon">
            <div class="title">
                <h2>{{posts.title}}</h2>
            </div>
            <ul class="taobar">
                <li>
                    发布于 {{posts.create_at | formatDate}}
                </li>
                <li v-if="posts.author">
                    作者 {{posts.author.loginname}}
                </li>
                <li>
                    {{posts.visit_count}} 次浏览
                </li>
                <li>最后一次编辑是 {{posts.last_reply_at | formatDate}}</li>
                <li>来自 {{posts | tabFormatter}}</li>
            </ul>
            <div id="content" v-html="posts.content"></div>
        </div>
        <div class="replycon" v-if="posts.reply_count">
            <ul>
                <li>{{posts.reply_count}}回复</li>
                <li v-for="( item , index ) in posts.replies" :key="item.id">
                    <span>
                        <router-link :to="{
                            name:'user_info',
                            params:{
                                id:item.id
                            }
                        }">
                            <img :src="item.author.avatar_url" alt="" srcset="">
                            <span>{{item.author.loginname}}</span>
                        </router-link>
                        {{index+1}}楼 {{item.create_at}}
                    </span>
                    <div id="content" v-html="item.content"></div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      posts: {}
    }
  },
  methods: {
    getData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
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

<style lang="scss">
@import url('../assets/markdown-github.css');
.article {
  flex-grow: 1;
  max-width: 890px;
  .maincon {
    margin-top: 12px;
    background-color: white;
    padding: 0 12px;
  }
  .title {
    padding-top: 4px;
  }
  .taobar {
    display: flex;
    font-size: 12px;
    color: rgb(131, 131, 131);
    border-bottom: 1px solid rgb(240, 240, 240);
    padding-bottom: 4px;
    > li {
      margin: 0 4px;
    }
    > li::before {
      content: '•';
    }
  }
  .replycon {
    background-color: white;
    margin-top: 12px;
    ul > li {
      padding: 8px;
      border-bottom: 1px solid rgb(240, 240, 240);
      span {
        font-size: 12px;
        img {
          width: 30px;
          height: 30px;
          vertical-align: middle;
        }
      }
    }
    ul > li:first-child {
      background-color: rgb(246, 246, 246);
      height: 40px;
      padding: 0 8px;
      display: flex;
      align-items: center;
    }
  }
}
</style>

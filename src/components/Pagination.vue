<template>
    <div class="Pagination">
        <button @click="goFirst">首页</button>
        <button @click="goPre">上一页</button>
        <button v-if="currentPage>3">...</button>
        <button v-for="(item) in pagebtns" :key="item.index" @click="jumpToPage(item)" :class="{'active':(item)===currentPage}">
            {{item}}
        </button>
        <button @click="goNext">下一页</button>
    </div>
</template>

<script>
export default {
  data() {
    return {
      pagebtns: [1, 2, 3, 4, 5, '...'],
      currentPage: 1,
      predisable: false
    }
  },
  methods: {
    jumpToPage(page) {
      if (page !== this.currentPage && page !== '...') {
        this.currentPage = page
        let index = this.pagebtns.indexOf(this.currentPage)
        if (index > 2) {
          for (let i = 1; i < index - 1; i++) {
            this.pagebtns.shift()
            this.pagebtns.splice(4, 0, this.pagebtns[3] + 1)
          }
        } else if (index < 2 && this.currentPage >= 3) {
          for (let i = 0; i < 2 - index; i++) {
            this.pagebtns.unshift(this.pagebtns[0] - 1)
            this.pagebtns.splice(5, 1)
          }
        }
        this.$emit('handlelist', this.currentPage)
      }
    },
    goPre() {
      if (this.currentPage > 1) {
        this.jumpToPage(this.currentPage - 1)
      }
    },
    goNext() {
      this.jumpToPage(this.currentPage + 1)
    },
    goFirst() {
      this.currentPage = 1
      this.pagebtns = [1, 2, 3, 4, 5, '...']
      this.$emit('handlelist', this.currentPage)
    }
  }
}
</script>

<style lang="scss" scoped>
.Pagination {
  display: flex;
  button {
    padding: 0.3em;
    min-width: 40px;
    background-color: white;
    border: 1px solid rgb(221, 221, 221);
  }
  button:focus {
    outline: none;
  }
  button:first-child {
    border-top-left-radius: 4px;
    border-bottom-left-radius: 4px;
  }
  button:last-child {
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
  }
  .active {
    background-color: black;
    color: white;
  }
}
</style>

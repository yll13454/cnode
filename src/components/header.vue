<template>
    <div class="header">
        <div class="wrapper">
            <div>
                <img src="../assets/cnodejs_light.svg" alt="" />
            </div>
            <ul class="navbar">
                <li>
                    <a href="">首页</a>
                </li>
                <li>
                    <a href="">新手入门</a>
                </li>
                <li>
                    <a href="">API</a>
                </li>
                <li>
                    <a href="">关于</a>
                </li>
                <li>
                    <a href="">注册</a>
                </li>
                <li>
                    <a href="">登陆</a>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.header {
  height: 50px;
  background-color: #444444;
  font-size: 14px;
  display: flex;
  justify-content: center;
  .wrapper {
    width: 90%;
    display: flex;
    justify-content: space-between;
  }
  img {
    width: 120px;
    height: 50px;
  }
  .navbar {
    display: inline-flex;
    align-items: center;
    > li {
      margin: 0 16px;
      &:last-child {
        margin-right: 0;
      }
      &:first-child {
        margin-left: 0;
      }
      a {
        color: #ccc;
        &:hover {
          color: white;
        }
      }
    }
  }
}
</style>

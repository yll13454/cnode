<template>
    <div id="app">
        <Header></Header>
        <div class="wrapper">
            <router-view name="main"></router-view>
            <router-view name="slidebar"></router-view>
        </div>
    </div>
</template>

<script>
import Header from '@/components/header'
export default {
  name: 'App',
  components: {
    Header
  }
}
</script>

<style>
body {
  margin: 0;
  background-color: rgb(225, 225, 225);
}
@media screen and (min-width: 900px) {
  .wrapper {
    padding: 0 80px;
  }
}
.wrapper{
    display: flex;
}
ul,
li,
a {
  list-style: none;
  text-decoration: none;
  padding: 0;
}
</style>

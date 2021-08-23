<template>
  <div v-if="Object.keys(commentInfo).length !== 0" class="comment-info">
    <div class="info-header">
      <span class="header-title">用户评价</span>
      <span class="header-more">更多</span>
    </div>

    <div class="info-user">
      <img :src="commentInfo.user.avatar" alt="">
      <span>{{commentInfo.user.uname}}</span>
    </div>

    <div class="info-detail">
      <p>{{commentInfo.content}}</p>
      <div class="info-other">
        <span class="date">{{commentInfo.created | showDate}}</span>
        <span>{{commentInfo.style}}</span>
      </div>
      <div class="info-imgs">
        <img v-for="(item, index) in commentInfo.images" :src="item" alt="" :key="index">
      </div>
    </div>
  </div>
</template>

<script>
  import {formatDate} from '../../../common/utils.js'

  export default {
    name: 'DetailCommentInfo',
    props: {
      commentInfo: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    filters: {
      showDate(value) {
        //1.将时间戳转成Date对象
        const date = new Date(value * 1000);
        //2.将date进行格式化
        return formatDate(date, "yyyy/MM/dd hh:mm:ss")
      }
    }
  }
</script>

<style scoped>
  .comment-info {
    border-bottom: 0.3rem solid rgba(128, 128, 128, 0.2);
  }
  .info-header {
    height: 50px;
    width: 310px;
    border-bottom: 1px solid rgb(192, 191, 191);
    margin: 0 auto;
  }
  .info-header span {
    display: inline-block;
    font-size: 14px;
    margin-top: 18px;
  }
  .header-more {
    position: absolute;
    right: 25px;
  }
  .header-title {
    position: absolute;
    left: 10px;
  }

  .info-user {
    margin-top: 5px;
    height: 50px;
  }
  .info-user img {
    height: 40px;
    width: 40px;
    border-radius: 50px;
    vertical-align: middle;
    margin-left: 10px;
  }
  .info-user span {
    display: inline-block;
    font-size: 14px;
    vertical-align: middle;
    margin-left: 10px;
  }

  .info-detail {
    padding: 0 10px;
  }
  .info-detail p {
    font-size: 12px;
    line-height: 17px;
    color: rgb(116, 115, 115);
  }
  .info-other {
    font-size: 12px;
    padding: 8px 0;
    color: rgb(150, 150, 150);
  }
  .info-other span {
    padding: 0 5px;
  }
  .info-imgs img{
    height: 50px;
    width: 50px;
    margin: 0 3px;
    margin-bottom: 10px;
  }
</style>
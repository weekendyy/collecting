<template>
  <div class="container">
    <el-carousel :interval="5000" arrow="none" height="250px">
      <el-carousel-item v-for="item in 3" :key="item">
        <img class="carouselItem" src="../assets/timing3.jpeg" />
      </el-carousel-item>
    </el-carousel>
    <div class="backBtn">
      <img class="backIcon" src="../assets/left.png" alt />
      <p>返回</p>
    </div>
    <div class="complain">投诉</div>
    <!-- 倒计时 -->
    <div class="timingBox">
      <p class="timingTxt">{{statusTxt}}：</p>
      <div class="timingNum">{{days}}</div>
      <p class="timingDesc">天</p>
      <div class="timingNum">{{hours}}</div>
      <p class="timingDesc">时</p>
      <div class="timingNum">{{minutes}}</div>
      <p class="timingDesc">分</p>
      <div class="timingNum">{{seconds}}</div>
      <p class="timingDesc">秒</p>
    </div>
    <!-- 活动内容-标题-数量 -->
    <div class="flexBox">
      <!-- 活动标题 -->
      <div class="activeInfo">
        <div class="activeTitle">
          <p>欢乐一下盛亚光翻个啊是的哈市空间欢乐一下盛亚光翻个啊是的哈</p>
        </div>
        <div class="bottomInfo">
          <div class="infoItem">
            <p class="itemName">已兑换：</p>
            <p class="itemNum">4份</p>
          </div>
          <div class="infoItem">
            <p class="itemName">剩余数量：</p>
            <p class="itemNum">4份</p>
          </div>
        </div>
      </div>
      <!-- 活动按钮 -->
      <div class="collectingBox">
        <p class="collectingTitle">您集卡 我送礼</p>
        <div class="lettersBox">
          <p class="letterItem" v-for="item in letters1" :key="item">{{item}}</p>
        </div>
        <div class="lettersBox">
          <p class="letterItem" v-for="item in letters1" :key="item">{{item}}</p>
        </div>
        <div class="btnBox">
          <p class="firstBtn">我要抽奖(剩余3次)</p>
          <p class="secondBtn">求助好友</p>
        </div>
      </div>
      <!-- 获奖名单 -->
      <div class="collectingBox" style="max-height: 270px;overflow: hidden">
        <p class="winnerTxt">获奖名单</p>
        <div class="winnerItem" v-for="(item ,index) in 4">
          <img src="../assets/timing3.jpeg" class="userIcon" />
          <div class="descBox">
            <p>想着光奔跑</p>
            <p>双人门票</p>
          </div>
          <p class="winnerTime">2019-06-06 13:43</p>
        </div>
      </div>
      <!-- 活动规则 -->
      <div class="collectingBox">
        <p class="winnerTxt">活动规则</p>
        <p v-for="(item, index) in rules">{{index+1}}、{{item}}</p>
        <p class="takeBtn">每次点击可领取5次</p>
      </div>
      <!-- 奖品介绍 -->
      <div class="collectingBox">
        <p class="winnerTxt">奖品介绍</p>
      </div>
    </div>

    <div style="height: 30px"></div>
  </div>
</template>

<script>
import numeral from "numeral";
export default {
  name: "Collecting",
  data() {
    return {
      CarouselImgs: [],
      letters1: ["清", "凉", "一", "夏"],
      letters2: ["清", "凉", "一", "夏"],
      startTime: 1567967857592,
      endTime: 1566997867592,
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      status: 1, //1表示活动开始倒计时  2表示活动结束倒计时 3表示活动已经结束
      statusTxt: "距离活动开始",
      rules: [
        "点击活动叶敏点击活动叶敏点击活动叶敏点击活动叶敏点击活动叶敏点击活动叶敏",
        "点击活动叶敏点击活动叶敏点击活动叶敏",
        "点击活动叶敏点击活动叶敏点击活动叶敏",
        "点击活动叶敏点击活动叶敏点击活动叶敏"
      ]
    };
  },
  methods: {
    TimeDown(startTime, endTime) {
      //当前时间
      let nowDate = new Date().getTime();
      //相差的总秒数
      let totalSeconds = 0;
      switch (this.status) {
        case 1:
          totalSeconds = parseInt((startTime - nowDate) / 1000);
          if (totalSeconds <= 0) {
            this.status = 2;
            this.statusTxt = "距离活动结束";
            this.TimeDown(startTime, endTime);
          }
          break;

        case 2:
          totalSeconds = parseInt((endTime - nowDate) / 1000);
          if (totalSeconds <= 0) {
            this.days = 0;
            this.hours = 0;
            this.minutes = 0;
            this.seconds = 0;
            this.status = 3;
            this.statusTxt = "活动已结束";
            return;
          }
          break;
        case 3:
          break;
      }

      //天数
      this.days = Math.floor(totalSeconds / (60 * 60 * 24));
      //取模（余数）
      let modulo = totalSeconds % (60 * 60 * 24);
      //小时数
      this.hours = Math.floor(modulo / (60 * 60));
      modulo = modulo % (60 * 60);
      //分钟
      this.minutes = Math.floor(modulo / 60);
      //秒
      this.seconds = modulo % 60;
      setTimeout(() => {
        this.TimeDown(startTime, endTime);
      }, 1000);
    }
  },
  mounted() {
    console.log(new Date().getTime());
    this.TimeDown(this.startTime, this.endTime);
  }
};
</script>

<style scoped lang="less">
.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  background: #e88042;
  overflow-x: hidden;
  .carouselItem {
    width: 100%;
  }
  .backBtn {
    width: 4rem;
    height: 2rem;
    border-bottom-right-radius: 5px;
    border-top-right-radius: 5px;
    font-size: 1rem;
    color: #023;
    background: #e88042;
    box-shadow: 3px 4px 25px 1px rgba(0, 0, 0, 0.8);
    position: fixed;
    top: 10px;
    left: 0;
    z-index: 10;
    line-height: 2rem;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    .backIcon {
      width: 16px;
    }
  }
  .complain {
    width: 50px;
    height: 50px;
    border-radius: 100%;
    border: 2px solid rgba(0, 0, 0, 0.3);
    font-size: 17px;
    color: #ece9cc;
    line-height: 50px;
    text-align: center;
    position: fixed;
    right: 10px;
    top: 10px;
    z-index: 10;
    background: rgba(0, 0, 0, 0.5);
    box-shadow: 0 5px 10px 3px rgba(0, 0, 0, 0.3);
  }
  .timingBox {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 6rem;
    color: #fff;
    .timingTxt {
      font-size: 13px;
    }
    .timingNum {
      width: 35px;
      height: 30px;
      background: #424242;
      font-weight: 600;
      font-size: 19px;
      text-align: center;
      border-radius: 0.3rem;
      line-height: 30px;
      box-shadow: 0 7px 7px -2px rgba(0, 0, 0, 0.8);
    }
    .timingDesc {
      font-size: 16px;
      margin: 0 3px;
    }
  }
  .flexBox {
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  .activeInfo {
    width: 92%;
    background: #fff;
    box-sizing: border-box;
    padding: 10px 30px;
    border-radius: 8px;
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 120px;
    &::after {
      content: "";
      width: 50px;
      height: 50px;
      background: #e88042;
      position: absolute;
      top: 35px;
      left: -30px;
      border-radius: 100%;
    }
    &::before {
      content: "";
      width: 50px;
      height: 50px;
      background: #e88042;
      position: absolute;
      top: 35px;
      right: -30px;
      border-radius: 100%;
    }
    .activeTitle {
      font-size: 16px;
      line-height: 1.5;
      text-align: center;
      font-weight: 600;
      margin-top: 0.2rem;
      height: 60px;
      p {
        font-weight: 600;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
      }
    }
    .bottomInfo {
      display: flex;
      align-items: baseline;
      justify-content: space-between;
      .infoItem {
        display: flex;
        align-items: baseline;
        .itemName {
          font-size: 0.9rem;
          color: #666;
        }
        .itemNum {
          font-size: 1.3rem;
        }
      }
    }
  }
  .collectingBox {
    width: 92%;
    background: #fff;
    margin-top: 20px;
    box-sizing: border-box;
    padding: 30px 20px;
    border-radius: 0.5rem;
    position: relative;
    &::before {
      content: "...........";
      font-size: 10rem;
      color: #e88042;
      position: absolute;
      top: -9.5rem;
      left: 0;
      letter-spacing: -0.2rem;
    }
    .collectingTitle {
      font-size: 1rem;
      font-weight: 600;
      color: #e67b49;
      text-align: center;
    }
    .lettersBox {
      display: flex;
      justify-content: space-around;
      margin-top: 20px;
      .letterItem {
        font-size: 42px;
        color: #babcc4;
        background: #f1f1f1;
        line-height: 1;
        padding: 10px;
        border-radius: 6px;
      }
    }
    .btnBox {
      display: flex;
      align-items: center;
      width: 100%;
      justify-content: space-between;
      margin-top: 30px;
      .firstBtn {
        background: #df584a;
        color: #fff;
        font-size: 16px;
        width: 60%;
        border-radius: 9px;
        padding: 10px 0;
        text-align: center;
      }
      .secondBtn {
        background: #df584a;
        color: #fff;
        font-size: 16px;
        width: 30%;
        border-radius: 9px;
        text-align: center;
        padding: 10px 0;
      }
    }
    .winnerTxt {
      color: #e28449;
      font-size: 17px;
      font-weight: 600;
      text-align: center;
      margin-bottom: 7px;
    }
    .winnerItem {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      &:last-child {
        margin-bottom: 0;
      }
      .userIcon {
        width: 40px;
        height: 40px;
        border-radius: 100%;
        margin-right: 10px;
      }
      .descBox {
        display: flex;
        flex-direction: column;
        font-size: 13px;
        color: #7b7b7b;
        flex: 1;
        line-height: 1.3;
      }
      .winnerTime {
        font-size: 12px;
        color: #7b7b7b;
      }
    }
    .takeBtn {
      color: #fff;
      font-size: 18px;
      text-align: center;
      margin-top: 20px;
      background: #df5748;
      padding: 7px;
      border-radius: 30px;
    }
  }
}
</style>

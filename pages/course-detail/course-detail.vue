<template>
  <view class="course-detail">
    <!-- 视频培训 -->
    <video
      v-if="item.mode==1"
      id="myVideo"
      :src="item.file"
      @error="videoErrorCallback"
      :show-center-play-btn="true"
      enable-danmu
      controls
      :initial-time="0.1"
      @play="play"
      @timeupdate="timeupdate"
    ></video>
    <view class="content">
      <view class="header">
        <image
          src="../../static/image/course-icon.png"
          mode=""
        />
        <text>课程介绍</text>
      </view>
      <view class="course-content">
        <view class="item">
          课程主题：
          <text class="theme">{{item.title}}</text>
          <!-- <text class="status">{{item.status}}</text> -->
        </view>
        <view class="item">培训时间：<text class="date">{{item.start_time}}-{{item.end_time}}</text></view>
        <view class="item">培训类型：<text class="type">
            {{item.mode==1?'视频':(item.mode==2?'课件':'直播')}}
          </text></view>
        <!-- <view class="item">参与名额：<text class="limit">{{item.limit}}</text>
					<text class="isReported">已有{{item.isReported}}人报名参加</text>
				</view> -->
        <view class="item item-last-child">
          课程介绍：<view class="desc">{{item.description}}</view>
        </view>
      </view>

      <!-- 显示课件PPT  -->
      <view
        class="courseware-content"
        v-if="item.mode===2"
      >
        <image
          :src="item.pic"
          v-for="(item,index) in coursewareList"
          :key="index"
        />
      </view>
      <!-- <view class="btn-area"><button type="primary" v-if="joined" @click="report" class="btn">我要报名</button></view> -->
      <Kefu />
      <Modal
        ref="modal"
        :status="modalStatus"
        :title="modal_title"
        :btn="false"
      >
        <view slot="desc">
          <view v-if="modalStatus == 'success'">{{modalMsg}}<text :style="{color:'rgba(243, 101, 35, 1)'}">{{integral}}</text>分</view>
          <view v-else>{{modalMsg}}</view>
          <!-- <view slot="desc">恭喜获得<text :style="{color:'rgba(243, 101, 35, 1)'}">10</text>积分</view> -->
        </view>
      </Modal>
    </view>
  </view>
</template>

<script>
import Kefu from "@/components/Kefu";
import Modal from "@/components/Modal";
export default {
  components: {
    Kefu,
    Modal,
  },
  data() {
    return {
      isGiveJfStatus: false, //赠送积分状态
      trainId: null,
      integral: 0, //积分
      modalMsg: null, // 提示信息
      modal_title: "",
      courseType: "视频",
      joined: false,
      item: {},
      modalStatus: "success",
      //课件
      coursewareList: [
        {
          pic: "../../static/image/kejian1.png",
        },
        {
          pic: "../../static/image/kejian1.png",
        },
      ],
    };
  },
  onLoad(props) {
    console.log(props);
    let id = props.id;
    this.trainId = props.id;
    let token = uni.getStorageSync("token");
    this.getDetail(id, token);

    this.joined = !props.joined;
    this.courseType = props.type;
  },
  methods: {
    getDetail(id, token) {
      id = Number(id);
      let that = this;
      uni.request({
        url: "https://amd.mcooks.cn/api/trained/show",
        method: "post",
        data: {
          id,
        },
        header: {
          authtoken: "token " + token,
        },
        success(res) {
          console.log("course---detail", res.data.data);
          if (res.data.code == 200) {
            that.item = res.data.data;
            uni.saveFile({
              tempFilePath: res.data.data.file,
              success: function (res) {
                console.log("TCL: success -> res", res);
              },
              fail: function (res) {
                console.log("TCL: fail -> res", res);
              },
              complete: function (res) {
                console.log("TCL: complete -> res", res);
              },
            });
            //
          }
        },
      });
    },
    // 赠送积分
    givejf() {
      return new Promise((resolve, reject) => {
        uni.request({
          url: "https://amd.mcooks.cn/api/call/integral",
          method: "post",
          data: {
            id: this.trainId,
          },
          header: {
            authtoken: "token " + uni.getStorageSync("token"),
          },
          success(res) {
            resolve(res);
          },
        });
      });
    },
    timeupdate(event) {
      let { currentTime, duration } = event.detail;
      // 超过2/3 就赠送积分
      if (currentTime / duration > 2 / 3) {
        if (!this.isGiveJfStatus) {
          this.isGiveJfStatus = true;
          this.givejf()
            .then((result) => {
              let { data } = result;
              if (data.code == 200) {
                this.modalStatus = "success";
                this.integral = data.data.integral;
                this.modalMsg = data.msg;
              } else {
                this.modalStatus = "error";
                this.modalMsg = data.msg;
              }
              if (this.modalMsg !== "您已获取过当前培训积分") {
                this.$refs.modal.open();
              }
            })
            .catch((err) => {});
        }
      }
    },
    videoErrorCallback(e) {},
    loadedmetadata(event) {
      console.log("TCL: loadedmetadata -> event", event);
      let { width, height, duration } = event.detail;
      console.log("TCL: loadedmetadata -> duration", duration);
    },
    play(e) {
      console.log("TCL: e", e);
    },
    // 我要报名
    report() {
      let token = uni.getStorageSync("token");
      if (token) {
        // 报名成功
        this.modal_title = "报名成功";
      } else {
        // 没有登录的
        this.modal_title = "无权参与，请先注册";
      }
      this.$refs.modal.open();
    },
  },
};
</script>

<style scoped lang="scss">
/deep/.uni-video-cover-duration {
  display: none;
}

/deep/.uni-video-cover {
  background-color: transparent;

  .uni-video-cover-play-button {
    position: relative;
    display: flex;
    align-items: center;
    justify-self: center;
  }

  .uni-video-cover-play-button::before {
    display: block;
    content: "";
    width: 88rpx;
    min-width: 88rpx;
    min-height: 88rpx;
    height: 88rpx;
    background: rgba(0, 0, 0, 1);
    opacity: 0.4;
    border-radius: 50%;
    z-index: -1;
    position: relative;
    left: -12%;
  }
}

.course-detail {
  height: calc(100vh - 88rpx);
  display: flex;
  flex-direction: column;
  justify-content: flex-start;

  #myVideo {
    height: 419rpx;
    width: 100%;
  }

  .content {
    position: relative;
    flex: 1;

    .course-content {
      padding: 0 30rpx 30rpx 30rpx;
      background: #fff;
    }

    .header {
      // margin-bottom: 30rpx;
      padding: 30rpx;
      background: #fff;

      image {
        width: 25rpx;
        height: 25rpx;
      }

      text {
        font-size: 28rpx;
        font-weight: bold;
        color: rgba(243, 101, 35, 1);
        line-height: 48rpx;
        margin-left: 10rpx;
        line-height: 1;
      }
    }

    .item {
      font-size: 26rpx;
      font-weight: 500;
      color: rgba(102, 102, 102, 1);
      margin-bottom: 30rpx;
      line-height: 1;

      .theme {
        font-size: 26rpx;
        font-weight: bold;
        color: rgba(51, 51, 51, 1);
		line-height: 34rpx;
      }

      .status {
        font-size: 22rpx;
        font-weight: 500;
        color: rgba(243, 157, 35, 1);
        float: right;
      }

      .date,
      .type {
        font-size: 26rpx;
        font-weight: 500;
        color: rgba(243, 101, 35, 1);
      }

      .limit {
        font-size: 26rpx;
        font-weight: 500;
        color: rgba(243, 157, 35, 1);
      }

      .isReported {
        font-size: 22rpx;
        font-weight: 500;
        color: rgba(255, 76, 76, 1);
        float: right;
      }

      .desc {
        margin-top: 30rpx;
        font-size: 26rpx;
        font-weight: 500;
        color: rgba(51, 51, 51, 1);
        line-height: 48rpx;
      }
    }

    .item-last-child {
      margin-bottom: 0 !important;
    }
  }

  .courseware-content {
    margin-top: 6rpx;
    padding: 30rpx;
    background: #fff;
    padding-bottom: 100rpx;

    image {
      width: 100%;
      margin-bottom: 30rpx;
    }
  }

  .btn-area {
    background: #fff;
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 100rpx;
    display: flex;
    align-items: center;

    .btn {
      height: 64rpx;
      line-height: 64rpx;
      background-color: rgba(243, 101, 35, 1);
      border-radius: 10rpx;
      width: calc(100% - 60rpx);
    }
  }
}
</style>
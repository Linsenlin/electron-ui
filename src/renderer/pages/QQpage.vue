<template>
  <Frame>
    <QQhead slot="header"></QQhead>
    <div class="bodydiv">
        <div class="firstDiv">
          <img  @mouseenter="openLeft($event, 0)" @mouseleave="closeLeft" src="http://p1.qzone.la/upload/20150102/a3zs6l69.jpg" class="userImg"></img>
          <div style="display:flex;flex-direction:column;margin-left:10px;width:100%">
            <span style="font-size:18px">我的qq</span>
            <span style="font-size:12px">只是测试一下</span>
          </div>
          <div style="padding-right:8px">
            <i class="rightIcon iconfont icon-yuncloud259" @mouseenter="showwindow"></i>
          </div>
        </div>
        <div class="seconddiv">
          <input type="text" style="width:100%;border: none;background: rgba(255, 253, 253, 0.2);height:30px;outline:none;padding: 0 10px;box-sizing:border-box">
        </div>
        <div style="display:flex;height:40px;align-items:center;justify-content:space-around">
          <span>消息</span>
          <span>联系人</span>
          <span>动态</span>
        </div>
        <div class="listdiv">
          <div class="itemdiv" v-for="(item, index) in list">
            <img @mouseenter="openLeft($event, index + 1)"  @mouseleave="closeLeft" src="http://p1.qzone.la/upload/20150102/a3zs6l69.jpg" class="itemImg"></img>
            <div style="display: flex;flex-direction:column;margin-left:10px">
              <span style="font-size:16px;font-weight:600">electron交流</span>
              <span style="font-size:12px">张三：发表信息</span>
            </div>
          </div>
        </div>
    </div>
    <QQFooter slot="footer"></QQFooter>
  </Frame>
</template>
<script>
import Frame from '@/components/frame/mainFrame.vue'
import QQhead from '@/components/header/QQhead.vue'
import QQFooter from '@/components/footer/QQFooter.vue'
// import { setTimeout } from 'timers'
export default {
  name: 'qq',
  data () {
    return {
      list: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      timeTap: ''
    }
  },
  methods: {
    openLeft (e, index) {
      // 查找有无窗口存在
      clearTimeout(this.timeTap)
      let width = 300
      let fatherBounds = this.$Win.win.getBounds()
      // 判断右边是否过界
      let leftWidth = e.screenX - e.offsetX - width - 15
      let x = leftWidth < 0 ? leftWidth + width + fatherBounds.width : leftWidth
      let y = e.screenY - e.offsetY
      let win = this.$Win.getWinByName('leftname')
      if (win) {
        this.$Win.routerPush({
          router: '/infoWindow/' + index,
          win: win,
          data: {name: index}
        })
        win.show()
        this.$Win.animation({
          win: win,
          time: 200,
          to: {
            x: x,
            y: y
          }
        })
      } else {
        this.$store.dispatch('changeTransition', 'default')
        win = this.$Win.createWin({
          width: 300,
          height: 200,
          windowConfig: {
            router: '/infoWindow/' + index,
            name: 'leftname',
            customAnimation: {
              fromPosition: {x: x, y: y - 50},
              time: 300 // 动画时间
            },
            data: {name: index},
            reuse: true,
            reload: true,
            vibrancy: true
          },
          x: x,
          y: y,
          alwaysOnTop: true,
          skipTaskbar: true
        })
        win.show()
      }
    },
    closeLeft () {
      clearTimeout(this.timeTap)
      let win = this.$Win.getWinByName('leftname')
      this.timeTap = setTimeout(() => {
        this.$Win.closeWin({}, win)
      }, 1000)
    },
    showwindow () {
      let fatherBounds = this.$Win.win.getBounds()
      // 判断右边是否过界
      let leftWidth = window.screen.width - fatherBounds.width - fatherBounds.x - 300
      let x = leftWidth >= 0 ? fatherBounds.width + fatherBounds.x : fatherBounds.x - 300
      let y = fatherBounds.y

      let win = this.$Win.createWin({
        width: 300,
        height: 200,
        x: x,
        y: y,
        windowConfig: {
          router: '/cloudWindow',
          vibrancy: false,
          name: 'cloud',
          animation: 'fromBottom'
        }
      })
      win.show()
    }
  },
  components: { Frame, QQhead, QQFooter }
}
</script>
<style>

.header {
    height: 60px;
    -webkit-app-region: drag;
}

.rightIcon {
    font-size: 30px;
    cursor: pointer;
}

.rightIconLast {
    padding-right: 5px;
}
.bodydiv{
  width:100%;
  height:100%;
  overflow: hidden;
}

.userImg{
	margin-left: 10px;
	width: 50px;
	height: 50px;
	border-radius: 50%;
	-webkit-app-region: no-drag;
	box-shadow: 0px 3px 13px 3px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}
.itemImg{
  margin-left: 10px;
	width: 40px;
	height: 40px;
	border-radius: 50%;
  cursor: pointer;
}
.firstDiv{
  display: flex;
  height: 75px;
  align-items:center
}
.itemdiv{
  display:flex;
  height: 60px;
  align-items:center

}
.itemdiv:hover{
  background: rgba(0, 0, 0, 0.1);
}
.listdiv{
  overflow: scroll;
  height: 100%;
}
</style>
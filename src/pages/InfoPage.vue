<template>
  <div>
    <el-row :gutter="20" class="mgb20">
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-1">
            <div class="grid-cont-right">
              <div class="grid-num">{{userCount}}</div>
              <div>用户总数</div>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-2">
            <div class="grid-cont-right">
              <div class="grid-num">{{songCount}}</div>
              <div>歌曲总数</div>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-3">
            <div class="grid-cont-right">
              <div class="grid-num">{{singerCount}}</div>
              <div>歌手数量</div>
            </div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="6">
        <el-card shadow="hover" :body-style="{padding: '0px'}">
          <div class="grid-content grid-con-4">
            <div class="grid-cont-right">
              <div class="grid-num">{{songListCount}}</div>
              <div>歌单数量</div>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <h3 style="margin-bottom: 20px">本周用户听歌平均时长</h3>
        <div class="cav-info" style="background-color : rgba(0,0,0,.4);">
          <ve-line :data="userUsing" :theme="options" ></ve-line>
        </div>
      </el-col>
      <el-col :span="12">
        <h3 style="margin-bottom: 20px">歌曲类型分布</h3>
        <div class="cav-info" style="background-color : rgba(0,0,0,.4);" >
          <ve-histogram :data="songStyle" :theme="options3"></ve-histogram>
        </div>
      </el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <h3 style="margin: 20px 0">用户性别比例</h3>
        <div class="cav-info" >
          <ve-pie :data="userSex" :theme="options1"></ve-pie>
        </div>
      </el-col>
      <el-col :span="12">
        <h3 style="margin: 20px 0">歌手国籍分布</h3>
        <div class="cav-info" >
          <ve-histogram :data="country" :theme="options2"></ve-histogram>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { mixin } from '../mixins'
import { HttpManager } from '../api/index'

export default {
  mixins: [mixin],
  data () {
    return {
      user: [],
      userUsing: {
        columns: ['日期', '听歌时长/小时'],
        rows: [
          {'日期': '周一', '听歌时长/小时': 1},
          {'日期': '周二', '听歌时长/小时': 3},
          {'日期': '周三', '听歌时长/小时': 2},
          {'日期': '周四', '听歌时长/小时': 4},
          {'日期': '周五', '听歌时长/小时': 5},
          {'日期': '周六', '听歌时长/小时': 6},
          {'日期': '周日', '听歌时长/小时': 8}]
      },
      userSex: {
        columns: ['性别', '总数'],
        rows: [
          { '性别': '男', '总数': 0 },
          { '性别': '女', '总数': 0 }
        ]
      },
      country: {
        columns: ['国家', '总数'],
        rows: [
          { '国家': '中国', '总数': 0 },
          { '国家': '韩国', '总数': 0 },
          { '国家': '意大利', '总数': 0 },
          { '国家': '新加坡', '总数': 0 },
          { '国家': '美国', '总数': 0 },
          { '国家': '马来西亚', '总数': 0 },
          { '国家': '西班牙', '总数': 0 },
          { '国家': '日本', '总数': 0 }
        ]
      },
      options: {
        axisLine: {
          show: true,
          textStyle: {
            color: ['#FFF']
          }
        },
        color: ['#FFF'],
        axisLabel: {
          show: true,
          textStyle: {
            color: ['#FFF']
          }
        }
      },
      options1: {
        color: ['#1E90FF', '#7B68EE']
      },
      options2: {
        color: ['#FEED78'],
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        }
      },
      options3: {
        color: ['#FFF'],
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        }
      },
      songStyle: {
        columns: ['分格', '总数'],
        rows: [
          { '分格': '华语', '总数': 0 },
          { '分格': '粤语', '总数': 0 },
          { '分格': '欧美', '总数': 0 },
          { '分格': '日韩', '总数': 0 },
          { '分格': 'BGM', '总数': 0 },
          { '分格': '轻音乐', '总数': 0 },
          { '分格': '乐器', '总数': 0 }
        ]
      },
      userCount: 0,
      songCount: 0,
      singerCount: 0,
      songListCount: 0
    }
  },
  mounted () {
    this.getUser()
    this.getSinger()
    this.getSong()
    this.getSongList()
  },
  methods: {
    getUser () {
      HttpManager.getAllUser().then(res => {
        this.userCount = res.length
        this.userSex.rows[0]['总数'] = this.setSex(1, res)
        this.userSex.rows[1]['总数'] = this.setSex(0, res)
      })
    },
    setSex (sex, arr) {
      let count = 0
      for (let item of arr) {
        if (sex === item.sex) {
          count++
        }
      }
      return count
    },
    getCountry (val) {
      for (let item of this.country.rows) {
        if (val.includes(item['国家'])) {
          item['总数']++
          break
        }
      }
    },
    getStyle (val) {
      for (let item of this.songStyle.rows) {
        if (val.includes(item['分格'])) {
          item['总数']++
        }
      }
    },
    getSinger () {
      HttpManager.getAllSinger().then(res => {
        this.singerCount = res.length
        for (let item of res) {
          this.getCountry(item.location)
        }
      }).catch(err => {
        console.log(err)
      })
    },
    getSong () {
      HttpManager.getAllSong().then(res => {
        this.songCount = res.length
      }).catch(err => {
        console.log(err)
      })
    },
    getSongList () {
      HttpManager.getSongList().then(res => {
        this.songListCount = res.length
        for (let item of res) {
          this.getStyle(item.style)
        }
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>

<style>
  .grid-content {
    display: flex;
    align-items: center;
    height: 100px;
  }

  .grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #999;
  }

  .grid-num {
    font-size: 30px;
    font-weight: bold;
  }

  .cav-info {
    border-radius: 6px;
    overflow: hidden;
  }
  .el-card{
    background-color: rgba(255,255,255,.1);
    box-shadow: 0px 2px 0px 0px rgba(0, 0, 0, 0.3);
  }
  .cav-info {
    background-color : rgba(255,255,255,.4);
  }
</style>

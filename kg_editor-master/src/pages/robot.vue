<template>
    <div class="container">
      <div class="title-container">
        <div class="title-center">
          <a class="title" href="https://github.com/g0ngjie/antv-x6-vue2">{{titleData.title}}</a>
        </div>
        <div class="title-buttons">
          <!-- <button class="title-button">
            <img src="http://localhost:5500/Temi-Program-Visualization-main/packages/icons/play.png"/>
          </button> -->
          <button class="title-button">
            <img class="title-button-img2" src="../../static/assets/play.png"/>
          </button>
        </div>
      </div>
      <div class="inner-container">
        <div class="episode-features">
            <label>User&Property: </label>
            <input id="user-property" type="text" placeholder="Feature 1">
            <label>Task objective: </label>
            <input id="task-objective" type="text" placeholder="Feature 2">
            <label>Style: </label>
            <input id="style" type="text" placeholder="Feature 3">
            <label>TimeStamp: </label>
            <input id="timestamp" type="text" placeholder="Feature 4">
        </div>
        <div class="questions">
            <input id="questions" type="text" placeholder="Question">
        </div>
        <div class="search-relate-node">
            <button @click="learnNewEpisode">Learn</button>
            <button @click="searchNewRelateNode">Search</button>
        </div>
        <div class="answers">
            <label>answers: </label>
            <label class="answers-content"></label>
        </div>
      </div>
    </div>
  </template>

<script>
import Vue from 'vue' // 导入 Vue 对象，用于创建响应式数据

export default {
  components: {
  },
  data () {
    return {
      titleData: Vue.observable({
        title: 'ROBOT UI'
      }),
      newTitleData: '',
      userProperty: '',
      taskObjective: '',
      style: '',
      timeStamp: '',
      question: '',
      socket: null
    }
  },
  mounted () {
    // 建立 WebSocket 连接到服务器
    // this.socket = new WebSocket('ws://localhost:8080')
  },
  methods: {
    learnNewEpisode () {
      // 先判断能否从该episode中学习得到新知识
      const userPropertyInput = document.getElementById('user-property')
      const taskObjectiveInput = document.getElementById('task-objective')
      const styleInput = document.getElementById('style')
      const timeStampInput = document.getElementById('timestamp')
      const questionInput = document.getElementById('questions')

      this.userProperty = userPropertyInput.value
      this.taskObjective = taskObjectiveInput.value
      this.style = styleInput.value
      this.timeStamp = timeStampInput.value
      this.question = questionInput.value

      // 学习过程

      // 学习结果--添加新的节点
      // 例如这里将userProperty节点作为新节点添加
      // const learnNodes = {label: this.userProperty, size: [140, 140], type: 'USER_PROPERTY', style: {fill: '#EDF8FB', stroke: '#EDF8FB', lineWidth: 1, shadowColor: '#909399', shadowBlur: 10, shadowOffsetX: 3, shadowOffsetY: 3}, labelCfg: {position: 'center', style: {fill: '#000000', fontWeight: 800, opacity: 1, fontFamily: '微软雅黑', fontSize: 18}}}
      // console.log('node.length: ', this.$store.state.dataList.nodes.length)
      let obj = {
        // id: String('node' + (this.$store.state.dataList.nodes.length + 1)),
        // label: String(this.$store.state.dataList.nodes.length + 1)
        // x: ev.x,
        // y: ev.y
        label: this.userProperty,
        size: [140, 140],
        type: 'USER_PROPERTY',
        style: {fill: '#EDF8FB', stroke: '#EDF8FB', lineWidth: 1, shadowColor: '#909399', shadowBlur: 10, shadowOffsetX: 3, shadowOffsetY: 3},
        labelCfg: {position: 'center', style: {fill: '#000000', fontWeight: 800, opacity: 1, fontFamily: '微软雅黑', fontSize: 18}}
      }
      // this.graph.addItem('node', obj)
      // this.$store.commit('addNode', obj)
      // // 操作记录
      // let logObj = {
      //   id: String('log' + (this.$store.state.log.length + 1)),
      //   action: 'addNode',
      //   data: obj
      // }
      // this.$store.commit('addLog', logObj)

      // 2. 发现更新的新节点的数据，然后通过 WebSocket 发送消息给服务器
      // const message = {
      //   type: 'updateDataList',
      //   payload: learnNodes
      // }
      // console.log('robot.vue: message: ', message)
      // this.socket.send(JSON.stringify(message))

      // 发起 WebSocket 连接
      const ws = new WebSocket('ws://localhost:8081')

      // 发送消息给 server.js，通知它更新 dataList
      ws.onopen = () => {
        const message = {
          type: 'updateDataList',
          payload: obj
        }
        ws.send(JSON.stringify(message))
        console.log('ws.send.updateDataList: ', message)
        ws.close()
      }
    },
    searchNewRelateNode () {
      this.learnNewEpisode()
      // search算法

      // 当前KG数据组
      console.log(this.$store.state.dataList)

      // 给出search结果
      // 在这里使用获取到的输入框的值进行处理，并将结果赋值给 answers 变量
      this.answers = '搜索结果' // 例如，这里将结果设置为字符串 "搜索结果"

      // 将结果填充到 answers-content 标签中
      const answersContentLabel = document.querySelector('.answers-content')
      answersContentLabel.textContent = this.answers
    }
  }
}

</script>

  <style>
  .test-button {
    position:absolute;
    left: 0px;
    top: 10px;
  }
  .container {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .inner-container {
    display: flex;
    flex: 1; /* take up all remaining space */
    height: 95%;
  }
  .title-container {
    display: flex;
    justify-content: space-between; /* 将内容左中右横向排布 */
    align-items: center; /* 垂直方向居中对齐 */
    text-align: center;
    padding: 3px;
    background-color: #5AB2B8;
    height: 5%;

  }
  .title-center {
    flex: 1; /* 自动占据剩余的空间 */
    text-align: center; /* 文字居中 */
  }
  .title-buttons {
    display: flex;
    justify-content: flex-end; /* 将按钮放置在容器的右侧 */
    align-items: center; /* 垂直居中按钮 */
  }
  .title-button {
    background-color: transparent;
    border: none;
    cursor: pointer;
    margin-right: 20px;
    /* width: 30px;
    height: 30px; */
  }
  /* 图片样式可以根据需要设置宽高等 */
  .title-button img {
    position: relative;
    top: 2px;
    width: 32px;
    height: 32px;
  }
  .title-button:hover{
    transform: scale(1.1) !important;
  }

  a.title {
    text-decoration: none;
    color: #FFF;
    font-family: Avenir;
    font-size: 18px;
    font-style: normal;
    font-weight: 800;
    line-height: normal;
    align-items: center; /* 垂直方向居中对齐标题 */
  }

  a.title:hover {
    text-decoration: underline;
  }
.inner-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; /* 添加此行代码 */
  text-align: center; /* 添加此行代码 */
  height: 100vh; /* 可选，使容器铺满整个视口 */
  width: 100%;
}

.episode-features,
.questions,
.search-relate-node {
  width: 100%;
  max-width: 600px;
  margin-bottom: 20px;
}

.episode-features {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 2000px;
}

.episode-features input[type="text"]{
  width: 100%;
  max-width: 180px;
  padding: 10px;
  margin: 0 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
.questions input[type="text"] {
  width: 100%;
  max-width: 500px;
  padding: 10px;
  margin: 0 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.search-relate-node button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
}

.search-relate-node button:hover {
  background-color: #0069d9;
}
.answers-content{
    color: red;
}
  </style>

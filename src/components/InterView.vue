<template>
  <div class="inter-view">

    <div class="welcome" v-show="interView.type==0">
      <h1>Dear Interviewors:<br/><br/>this is the index page for my interview. if you want to visit the original website of demo you can click
          <a href="#!/news/1">here</a>, but if you want to know me more, you need to click <a href="#!/inter-view/intro">this</a>
      </h1>
    </div>

    <div class="intro" v-show="interView.type==1">
      <h2>The interviewee info:</h2>
      <p>name:<span>{{interView.name}}</span></p>
      <p>age:<span>{{interView.age}}</span></p>
      <p>what he want to say:<span>{{interView.desc}}</span></p>
      <button type="button" name="button" v-on:click="showMoreDetail">if you just dont satisfied like this, touch me!</button>
    </div>

    <div class="more-detail" v-show="interView.type==2">
      <h2>{{interView.desc}}</h2>
      <ul v-for="item in interView.items">
        <li>
          <div>
            <a href="{{item.url}}" target='_blank' @click="thx">{{item.name}}</a>
            tech contained:<span class="detail-tech" v-html="item.withTech | splitToBlock"></span>
          </div></li>
      </ul>
  </div>
</template>

<script>
import store from '../store'

export default {

  name: 'InterView',

  data () {
    return {
      interView: {
        type:1
      }
    }
  },

  route: {
    data ({ to }) {
      document.title = 'InterView: ' + to.params.id + ' | jyjin\'s interview test'
      return {
        interView: store.fetchInterView(to.params.id)
      }
    }
  },

  methods:{
    showMoreDetail(){
      location.href = '/#!/inter-view/moreDetail'
    },
    thx(el){
      if(el.target.getAttribute('href') == 'http://www.easyar.cn'){
        alert('Thanks for your watch, all test over!');
        location.href = '/'
      }
    }
  },

  filters:{
    splitToBlock(techStr){
      let arr = techStr.split(',');
      let newTechStr = '';
      for (let i = 0, l = arr.length; i < l; i++) {
        newTechStr += `<i>${arr[i]}</i>`;
      }
      return newTechStr
    }


  }
}
</script>

<style lang="stylus">
@import "../variables.styl"

.inter-view
  color $gray

  a
    color $blue
    text-decoration underline
  .intro
    span
      background-color $white
      border-radius 5px
      padding 5px 10px

.more-detail
  li
    margin-bottom 5px
    a
      display inline-block
      margin-right 20px
  i
    display inline-block
    background-color $white
    padding 0px 5px
    border-radius 3px
    margin-right 5px
</style>

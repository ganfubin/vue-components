<template>
  <layout>
    <x-header slot="header">
      <div slot="title">Flow</div>
    </x-header>
    <x-body slot="body" :scroll="false" class="demos flow-demos">
      <flow @on-pullup="pullupHandler" @on-pulldown="pulldownHandler" :loading="loading">
        <template v-for="item in list">
          <div class="flexbox">
            <x-img class="avator" :src="item.author.avatar_url" />
            <div class="flex-item">
              <h4>{{item.title}}</h4>
              <div>
                {{new Date().toLocaleString()}}
              </div>
            </div>
          </div>
          <divider></divider>
        </template>
      </flow>
    </x-body>
  </layout>
</template>

<script>
import {
  Layout,
  XHeader,
  XBody,
  Flow,
  Divider,
  XImg
} from '../components'

import api from '../api'

let lastPage = 1
export default {
  components: {
    Layout,
    XHeader,
    XBody,
    Flow,
    Divider,
    XImg
  },
  mounted () {
    this.fetch()
  },
  methods: {
    pullupHandler (e) {
      this.fetch(lastPage + 1)
    },
    pulldownHandler (e) {
      this.fetch(1)
    },
    fetch (page = 1, cb) {
      let bool = page > lastPage
      if (bool) {
        lastPage = page
      }
      this.loading = true
      api.cnode.list({page: page, tab: ''}).then((data) => {
        this.loading = false
        if (bool) {
          this.list = this.list.concat(data.data)
        } else {
          this.list = data.data.concat(this.list)
        }
        cb && cb()
      })
    }
  },
  data () {
    return {
      list: [],
      loading: true
    }
  }
}
</script>

<style lang="scss">
  .flow-demos .v-flow{
    height:100%;
    background:#fff;
    position: absolute;
    width: 100%;
  }
  .flow-demos{
    .flexbox{
      display:box;
      display:flex;
      align-items: center;
      padding:10px 0px;
    }
    .flex-item{
      flex:1;
      padding:0px 10px;
    }
    .avator{
      width:50px;
      height:50px;
      margin-left:10px;
    }
    h4{
      margin:0;
    }
  }
</style>

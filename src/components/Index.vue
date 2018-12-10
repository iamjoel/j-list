<template>
  <div class="data-fetcher">
    <van-list
      v-model="isLoading"
      :finished="isFinished"
      @load="fetchList"
    >
      <template v-for="(item, i) in list">
        <slot :data="item" :index="i"/>
      </template>
    </van-list>
    <div v-if="!isLoading && list.length === 0" class="ta-c lh-lg mt-10rem">
      暂无数据
    </div>
  </div>
</template>

<script>
import { List, Toast } from 'vant'
import axios from 'axios'

export default {
  props: {
    fetchListFn: null,
    url: String,
    searchCondition: {
      type: Object,
      default: () => {return {}}
    },
    order: Array,
    pageLimit: {
      type: Number,
      default: 5
    },
    isInfinate: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      list: [],
      isLoading: false,
      isFinished: false,
      pager: {
        current: 0,
        total: 1,
      },
    }
  },
  methods: {
    fetchList(isReSearch) {
      if(!this.validParams()) {
        return
      }
      if(!isReSearch) {
        if(this.isFinished) {
          return
        }
        this.pager.current++
      } else {
        this.isFinished = false
        this.pager.current = 1
      }
      
      this.isLoading = true
      
      // 非无限加载的，只调用一次接口
      if(!this.isInfinate) {
        this.isFinished = true
      }
      fetchList(this.url, this.searchCondition, this.pager, this.order).then(({data}) => {
        this.list = isReSearch ? data.data : this.list.concat(data.data)
        this.pager.total = data.pager.total
        if(this.pager.current >= Math.ceil(data.pager.total / this.pageLimit)) {
          this.isFinished = true
        }
        this.isLoading = false
      })

    },
    validParams(isShowError) {
      var errMsg
      if(!this.url) {
        errMsg = '接口地址必传'
      }
      if(errMsg) {
        if(isShowError) {
          Toast(`j-list: ${errMsg}`)
        }
        return false
      } else {
        return true
      }
    }
    
  },
  mounted() {
    if(!this.validParams(true)) {
      return
    }
    this.pager.item = this.pageLimit
    this.fetchList()
  }
}

function  fetchList(urlPrefix, condition, pager, order, axiosConfig={}) {
  var url = wrapFetchQuery(urlPrefix,
    condition,
    pager,
    order
  )
  return axios.get(url, {
    params: {
    },
    ...axiosConfig
  })
}

function wrapFetchQuery(url, query, pager, order) {
  var res = url
  if (query || pager) {
    res += '?'
    var hasQuery = false
    if (query) {
      let validQuery = {}
      for (let key in query) {
        if (query.hasOwnProperty(key) && query[key] !== '' && query[key] !== undefined && query[key] !== null) {
          validQuery[key] = encodeURIComponent(query[key])
          hasQuery = true
        }
      }
      if(hasQuery) {
        res += ('where=' + JSON.stringify(validQuery))
      }
    }

    if (pager) {
      if (hasQuery) {
        res += '&'
      }
      res += (`pageAt=${pager.current}&`)
      res += (`pageLimit=${pager.item || 10}`) // 一页几条
    }

    if(order) {
      res += ('&order=' + encodeURIComponent(JSON.stringify(
        [order] // order 类似 ['name', 'desc或asc']
      )))
    }
  }
  return res
}
</script>


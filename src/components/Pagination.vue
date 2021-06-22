<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="isEllipsis" class="pageBtn">...</button>
    <button v-for="(btn, index) in pageButtons" :key="index"
            :class="[{isCurrentPage: (btn === currentPage)}, 'pageBtn']"
            @click="changeBtn(btn)">
      {{btn}}
    </button>
    <button class="pageBtn">...</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'Pagination',
  data(){
    return {
      pageButtons: [1,2,3,4,5],
      currentPage: 1,
      isEllipsis: false
    }
  },
  methods: {
    changeBtn(value){
      // 首页、上一页、下一页
      if((typeof value) !== 'number'){
        switch (value.target.innerText){
          case '上一页':
            $('button.isCurrentPage').prev().click()
            break;
          case '下一页':
            $('button.isCurrentPage').next().click()
            break;
          case '首页':
            this.pageButtons = [1,2,3,4,5]
            this.changeBtn(1)
            break;
          default:
            break;
        }
        return
      }
      // 数字页
      this.currentPage = value
      if(value === this.pageButtons[4]){
        this.pageButtons.shift()
        this.pageButtons.splice(4, 0, this.pageButtons[3]+1)
      }else if(value === this.pageButtons[0] && value !== 1){
        this.pageButtons.pop()
        this.pageButtons.splice(0,0,this.pageButtons[0]-1)
        // this.pageButtons.unshift(this.pageButtons[0]-1)
        // this.pageButtons.splice(5, 1)
      }
      // 省略
      // 简化：this.isEllipsis = value > 4;
      if(value > 4){
        this.isEllipsis = true
      } else {
        this.isEllipsis = false
      }
      this.$emit('handleList', this.currentPage)
    }
  }
}
</script>

<style scoped>
.pagination {
  margin-top: 5px;
  margin-bottom: 20px;
  background-color: #fff;
  padding: 6px 20px;
  border-radius: 5px;
  /*border: 1px solid #888888;*/
  text-align: center;
}

button {
  background-color: #fff;
  border: 1px solid #ddd;
  color: #778087;
  border-radius: 3px;
  outline: none;
  /*height: 21px;*/
  cursor: pointer;
  padding: 0 2px;
  width: 55px;
  height: 29px;
}
button:hover {
  background-color: #63605f;
}

.pageBtn {
  position: relative;
  bottom: 1px;
  width: 40px;
  margin: 0 4px;
}

.isCurrentPage {
  color: #fff;
  background-color: #63605f;
}
</style>

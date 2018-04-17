<template>
  <div class="page-wrapper clearfix">
      <div class="page-info fl">
          <span class="item-count h50">总共<span>{{totals}}</span>条，</span>
          <span class="h50"><span>{{totalPages}}</span>页</span>
      </div>
      <div class="page-tab fl clearfix">
          <button class="fl h50 cursor" :class="{canNot:currentPage==1}" @click="firstPage" :disabled='preDisabled'>首页</button>
          <button class="fl h50 cursor" :class="{canNot:currentPage==1}" @click="prePage" :disabled='preDisabled'>上一页</button>
          <ul class="fl">
              <li v-for="(item,index) in itemArr" :key="index" class="cursor" @click="changePage(item)" :class="{activePage:currentPage=== item}">{{item}}</li>
          </ul>
          <button class="fl h50 cursor" @click="nextPage" :class="{canNot:currentPage==totalPages}" :disabled='nextDisabled'>下一页</button>
          <button class="fl h50 cursor" :class="{canNot:currentPage==totalPages}" :disabled='nextDisabled' @click="lastPage">尾页</button>
      </div>
      <div class="items-choose fl clearfix">
          <span class="fl h50">每页</span>
          <div class="items-show fl" @click="handleChooseNumClick"><span class="h50">{{pageNum}}</span>
                <div class="per-page-items">
                    <!-- <input type="text" class="input-item-num"> -->
                    <ul class="items-num" v-if="itemsShow">
                        <li @click.stop='chooseNum($event)'>5</li>
                        <li @click.stop='chooseNum($event)'>10</li>
                        <li @click.stop='chooseNum($event)'>15</li>
                        <li @click.stop='chooseNum($event)'>20</li>
                    </ul>
                </div>
          </div>
      </div>
  </div>
</template>
<style>
clearfix:after{
    content: '.';
    height: 0;
    display: block;
    visibility: hidden;
    clear:both;
    overflow:hidden;
}
.cursor{
    cursor: pointer;
}
clearfix{
    zoom:1;
}
.page-wrapper .fl{
    float:left;
}
.h50{
    display: inline-block;
    height:40px;
    line-height:40px;
    padding:0 12px;
    border:1px solid #EAEDF1;
    border-radius:3px;
}
.page-wrapper .page-tab li{
    float:left;
    width:40px;
    height:40px;
    text-align:center;
    line-height:40px;
    border:1px solid #EAEDF1;
    border-radius:3px;
    box-sizing: border-box;
}
.page-wrapper .page-info{
    margin-right:6px;
}
.page-wrapper .page-info .h50{
    border:none;
    padding:0;
}
.items-choose .h50{
    padding:0;
    border:none 0;
    margin-left:6px;
}
.items-choose .items-show{
    height:40px;
    width:74px;
    position: relative;
    box-sizing:border-box;
    border:1px solid #EAEDF1;
    margin-left:6px;
    position: relative;
}
.items-choose .items-show:after{
    content: '';
    position: absolute;
    height:0;
    border:4px solid transparent;
    border-top:6px solid #C4CCC5;
    top:50%;
    right:10px;
    transform:translate3d(-50,-50,0);
}
.items-choose .items-num{
    width:100%;
    position: absolute;
    bottom:42px;
    border:1px solid #EAEDF1;
    z-index: 100;
    background:#fff;
}
.items-choose .items-num li{
    padding:10px 0 10px 6px;
}
.items-choose .items-num li:hover{
    background:#1AB394;
    color:#fff;
}
.page-wrapper .activePage{
    color:#fff;
    background:#1AB394;
}
.canNot{
    /* pointer-events: none; */
    cursor: not-allowed;
}
.page-wrapper button{
    background:#fff;
}
</style>
<script>
export default {
    name: 'VuePagination',
    props: {
        pageSize: {
            default: 5,
            type: Number
        },
        totals: {
            default: 40,
            type: Number
        }
    },
    data () {
        return {
            itemsShow: false, // 控制每页条数下拉框
            itemArr: [], // 显示页数,
            currentPage: 1, // 用来控制当前选中页数的样式,
            nextDisabled: null,
            preDisabled: 'disabled',
            pageNum: this.pageSize

        };
    },
    created () {
        this.pages();
    },
    methods: {
        chooseNum (evt) {
            this.itemsShow = false;
            this.pageNum = evt.target.innerHTML;
        },
        handleChooseNumClick () {
            this.itemsShow = !this.itemsShow;
        },
        changePage (page) { // 切换页数
            this.currentPage = page;
            this.pages();
        },
        nextPage () {
            if (this.currentPage <= this.totalPages - 1) {
                this.currentPage++;
                this.pages();
            }
        },
        prePage () {
            if (this.currentPage > 1) {
                this.currentPage--;
                this.pages();
            }
        },
        firstPage () {
            this.currentPage = 1;
            this.pages();
        },
        lastPage () {
            this.currentPage = this.totalPages;
            this.pages();
        },
        pages () {
            this.itemArr = [];
            this.totalPages = Math.ceil(this.totals / this.pageNum);
            this.preDisabled = this.currentPage === 1 ? 'disabled' : null;
            this.nextDisabled = this.currentPage === this.totalPages ? 'disabled' : null;
            let start = this.currentPage - 2 > 1 ? this.currentPage - 2 : 1;
            let end = this.currentPage > 3 ? (this.totalPages - this.currentPage >= 2 ? this.currentPage + 2 : this.totalPages) : 5;
            start = this.totalPages - this.currentPage >= 2 ? start : end - 4;
            if (this.totalPages <= 5) {
                start = 1;
                end = this.totalPages;
            }
            for (let i = start;i <= end;i++) {
                this.itemArr.push(i);
            }
        }
    },
    watch: {
        pageNum () {
            this.currentPage = 1;
            this.pages();
        }
    }
};
</script>

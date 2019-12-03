<template>
    <ul class="pagination">
        <li class="arrowL" @click="prePage" v-if="all && cur !== 1">
            <i class="icon-black icon-ArrowLeft"></i>
        </li>
        <li v-for="num in numArr"
            v-if="all !== 1"
            :class="{ 'page-break': num === '...', 'page-cur': num === cur}"
            @click="num === '...' ?  '' : handlePageClick(num)">
            <span>{{num}}</span>
        </li>
        <li class="arrowR" @click="nextPage" v-if="all && cur !== all">
            <i class="icon-black icon-ArrowRight"></i>
        </li>
    </ul>
</template>

<script>
    export default {
        name: "pagination",
        data() {
            return {
                cur: this.pageNo//当前页码数
            }
        },
        props: {
            pageNo: Number,//当前页码数
            all: Number//页码总数
        },
        watch:{
            pageNo(val) {
                this.cur = val
            }
        },
        computed: {
            numArr() {
                let self = this;
                let numArr = [];
                if(self.all < 6 && self.all > 0 ){
                    for(let i = 1; i <= self.all; i++){
                        numArr.push(i)
                    }
                }else if(self.all >= 6){
                    //判断是否有前一个的省略号...
                    let maxNum = self.cur + 2 >= self.all ? self.all - 1 : self.cur + 2;
                    if(self.cur > 5 ){
                        numArr.push(1);
                        numArr.push('...');
                        for(let i = this.cur - 3; i <= maxNum; i++){
                            numArr.push(i)
                        }
                    }else {
                        for(let i = 1; i <= maxNum; i++){
                            numArr.push(i)
                        }
                    }
                    //判断是否有后一个的省略号...
                    if(self.all - self.cur > 3)
                        numArr.push('...');
                    numArr.push(self.all)
                }
                return numArr
            }
        },
        methods: {
            prePage() {
                (this.cur -= 1) < 1 ? this.cur = 1 : this.cur
                this.$emit('changePage', this.cur)
            },
            nextPage() {
                (this.cur += 1) > this.all ? this.cur = this.all : this.cur
                this.$emit('changePage', this.cur)
            },
            handlePageClick(num) {
                this.cur = num
                this.$emit('changePage', num)
            }
        }
    }
</script>

<style lang="scss" scoped>

    ul.pagination {
        overflow: hidden;
        padding: 0;
        margin: 0;
        li {
            float: left;
            min-width: 36px;
            height: 36px;
            line-height: 36px;
            text-align: center;
            margin-right: 10px;
            color:  #5C5C5C;
            font-size: 14px;
            border: 1px solid #EDEDED;
            background: #fff;
            border-radius: 6px;
            cursor:pointer;
            box-sizing: border-box;
            position: relative;
            span{
                position: relative;
            }
        }
        li:not(.page-break,.page-cur):hover {
            border: 1px solid #4DB46A;
        }
        .page-cur {
            background: #4DB46A;
            color: #fff;
            border: 0;
        }
        .page-cur:hover {
            background: #5ED27F;
            border: 0;
            &:before{
                opacity: 1;
            }
        }
        .page-cur:before {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            top: 50%;
            left:  50%;
            opacity: 0;
            background: #5ED27F;
            transform: translate(-50%, -50%);
            transition: opacity .3s

        }
        .page-break {
            &:hover{
                border: 0;
            }
            font-weight: $font-weight-bold;
            color: #383838;
            border: 0;
        }
        .no-border{
            border: 1px solid #EDEDED;
        }
        .arrowL:active {
            background: #4DB46A;
            border: 0;
            i{
                color: white;
            }
        }
        .arrowR:active {
            background: #4DB46A;
            border: 0;
            i{
                color: white;
            }
        }
    }


</style>
<template>
    <div class="page-box" v-show="total > 1">
        <div class="page">
            <div class="pre" :class="{enable: pre}" @click="prePage">上一页</div>
            <ul class="page-list">
                <li class="page-item" :class="{'page-more': p.type == 'more', 'active': p.text == index}" v-for="p in pages" @click="changePage(p)">
                    {{p.text}}
                </li>
            </ul>
            <div class="next" :class="{enable: next}" @click="nextPage">下一页</div>
        </div>
        <div class="input-page">
            <span>跳转到第</span><input type="number" v-model="input"><span>页</span>
            <div class="confirm" @click="pageInput">确定</div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "VuePagination",
        props: {
            //页码总数 必需 大于0
            total: {
                type: Number,
                required: true,
                validator: function (value) {
                    return value > 0;
                }
            },
            //当前显示页码
            current: {
                type: Number,
                default: 1
            },
            //可显示页码 奇数 大于等于5
            show: {
                type: Number,
                default: 7,
                validator: function (value) {
                    return value % 2 > 0 && value >= 5;
                }
            }

        },
        data () {
            return {
                index : this.current,
                input: null
            }
        },
        computed: {
            pre () {
                return this.index > 1
            },
            next () {
                return this.index < this.total
            },
            pages () {
                let list;
                let total = this.total;
                let show = this.show;
                if(total <= show) {
                    list = [];
                    for(let i = 0; i < total; i ++) {
                        list.push({text: i + 1, type: 'num'});
                    }
                } else {
                    list = new Array(show);
                    list[0] = {text: 1, type: 'num'};
                    list[show - 1] = {text: total, type: 'num'};
                    let index = this.index;
                    let more = {text: '...', type: 'more'};
                    let ss = show - 2;
                    let es = total - ss;
                    if(index <= ss) {
                        for(let i = 1; i <= ss; i ++) {
                            list[i] = {text: i + 1, type: 'num'};
                        }
                        list[show - 2] = more;
                    } else if(index > es) {
                        for(let i = show - 2; i > 1; i --) {
                            list[i] = {text: i + 1 + total - show, type: 'num'};
                        }
                        list[1] = more;
                    } else {
                        list[1] = more;
                        list[show - 2] = more;
                        let half = (show + 1) / 2;
                        for(let i = 2; i < show - 2; i ++) {
                            list[i] = {text: index + i + 1 - half, type: 'num'};
                        }
                    }
                }
                return list;
            }
        },
        watch: {
            index: function (val) {
                this.$emit("change", val);
            },
            input: function (val) {
                if(val <= 0) {
                    this.input = 1;
                } else if (val > this.total) {
                    this.input = this.total;
                }
            }
        },
        methods: {
            pageInput () {
                let num = this.input * 1;
                num && (this.index = num * 1);
            },
            prePage () {
                this.pre && (this.index -= 1);
            },
            nextPage () {
                this.next && (this.index += 1);
            },
            changePage (page) {
                if(page.type == 'more') {
                    return;
                }
                this.index = page.text;
            }
        }
    }
</script>

<style scoped lang="less">
    .page-box {
        font-size: 0;
    }
    .input-page {
        display: inline-block;
        vertical-align: middle;
        margin-left: 10px;
        font-size: 0;
        >* {
            display: inline-block;
            vertical-align: middle;
            height: 28px;
            line-height: 26px;
            font-size: 12px;
            color: #cfcfcf;
        }
        input {
            display: inline-block;
            vertical-align: middle;
            margin: 0 5px;
            width: 28px;
            background: #fff;
            border-radius: 0;
            border: 1px solid #dcdcdc;
            -webkit-appearance: none;
            color: #333;
            text-align: center;
        }
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
        }
        input[type="number"]{
            -moz-appearance: textfield;
        }
        .confirm {
            display: inline-block;
            vertical-align: middle;
            margin-left: 5px;
            width: 40px;
            border: 1px solid #dcdcdc;
            cursor: pointer;
            text-align: center;
            &:hover {
                color: #333;
            }
        }
    }
    .page {
        display: inline-block;
        vertical-align: middle;
        border: 1px solid #dcdcdc;
        font-size: 0;
    }
    .page > div,
    .page > ul {
        display: inline-block;
        vertical-align: middle;
        font-size: 12px;
    }
    .pre,
    .next{
        width: 60px;
        color: #cfcfcf;
        height: 26px;
        line-height: 26px;
        text-align: center;
        cursor: pointer;
        &.enable:hover,
        &.enable:active {
            background-color: #851e23;
            color: #fff;
        }
    }
    .next {
        border-left: 1px solid #dcdcdc;
    }
    .page-list {
        font-size: 0;
    }
    .page-item {
        display: inline-block;
        vertical-align: middle;
        width: 27px;
        height: 26px;
        border-left: 1px solid #dcdcdc;
        font-size: 12px;
        line-height: 26px;
        text-align: center;
        cursor: pointer;
        &:hover,
        &:active,
        &.active{
            background-color: #851e23;
            color: #fff;
        }
        &.page-more {
            font-weight: bold;
        }
    }
</style>

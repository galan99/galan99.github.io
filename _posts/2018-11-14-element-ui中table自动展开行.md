---
layout: post
published: true
title: Element UI 中table自动展开行
category: web
tags: 
  - [vue,element-ui]
excerpt: 在使用vue版本的ElementUI中的table功能的时候还是遇到了一些问题，可以说饿了么团队在这个UI框架的文档撰写已经非常不错了，不过还是有一些方法乍一看让人摸不着头脑，有些table的常用功能示例代码提供的不是非常详细

---

# table自动展开，以及某些行需要展开，某些行不需要展开

<br/>
<br/>
自动展开：<br/>
核心是 row-key、expand-row-keys属性<br/>
特别要注意的是row-key传入的是一个Function(row)，而expand-row-keys传入的是一个数组，元素的值是要展开的row的key。
<br/>
<br/>

某些行需要展开，某些行不需要展开：<br/>
:row-class-name="setClassName", 行的 className 的回调方法，也可以使用字符串为所有行设置一个固定的 className<br/>
<br/>
<br/>

```javascript

<template lang="html">
    <el-table
        :data="tableData5"
        :row-key="getRowKeys"
        :expand-row-keys="expands"
        :row-class-name="setClassName"
        style="width: 100%">
        <el-table-column type="expand">
            <template scope="props">
                <el-form label-position="left" inline class="demo-table-expand">
                    <el-form-item label="商品名称">
                        <span>{{ props.row.name }}</span>
                    </el-form-item>
                    <el-form-item label="所属店铺">
                        <span>{{ props.row.shop }}</span>
                    </el-form-item>
                    <el-form-item label="商品 ID">
                        <span>{{ props.row.id }}</span>
                    </el-form-item>
                    <el-form-item label="店铺 ID">
                        <span>{{ props.row.shopId }}</span>
                    </el-form-item>
                    <el-form-item label="商品分类">
                        <span>{{ props.row.category }}</span>
                    </el-form-item>
                    <el-form-item label="店铺地址">
                        <span>{{ props.row.address }}</span>
                    </el-form-item>
                    <el-form-item label="商品描述">
                        <span>{{ props.row.desc }}</span>
                    </el-form-item>
                </el-form>
            </template>
        </el-table-column>
        <el-table-column
            label="商品 ID"
            prop="id">
        </el-table-column>
        <el-table-column
            label="商品名称"
            prop="name">
        </el-table-column>
        <el-table-column
            label="描述"
            prop="desc">
        </el-table-column>
    </el-table>
</template>

<script>
export default {
    data() {
        return {
            tableData5: [{
                id: '12987122',
                name: '好滋好味鸡蛋仔',
                category: '江浙小吃、小吃零食',
                desc: '荷兰优质淡奶，奶香浓而不腻',
                address: '上海市普陀区真北路',
                shop: '王小虎夫妻店',
                shopId: '10333'
            }, {
                id: '12987123',
                name: '好滋好味鸡蛋仔',
                category: '江浙小吃、小吃零食',
                desc: '荷兰优质淡奶，奶香浓而不腻',
                address: '上海市普陀区真北路',
                shop: '王小虎夫妻店',
                shopId: '10333'
            }, ],
            // 获取row的key值
            getRowKeys(row) {
                return row.id;
            },
            // 要展开的行，数值的元素是row的key值
            expands: []
        }
    },
    mounted() {
        // 在这里你想初始化的时候展开哪一行都可以了
        this.expands.push(this.tableData5[1].id);
    },
    methods:{
        setClassName(row, index){
            // 通过自己的逻辑返回一个class或者空
            return row.reply ? '' : 'expandno';
        },
    }
}
</script>

<style lang="css">
.demo-table-expand {
   font-size: 0;
}
.demo-table-expand label {
    width: 90px;
    color: #99a9bf;
}
.demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 50%;
}
.expandno .el-table__expand-column .cell {
    display: none;
}
</style>

```











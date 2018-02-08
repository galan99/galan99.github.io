---
layout: post
published: true
title: 解决element-ui 中upload组件使用多个时无法绑定对应的元素,以及上传前限制图片大小，格式以及尺寸
category: web
tags: 
  - [vue]
excerpt: 我们在一个列表中分别都需要有upload组件的时候也就涉及到了多个upload同时存在，可以在success回调中拿到上传成功的图片已经成功的response，多个也可以，但是，当多个同类型的upload同时存在的时候，怎么知道回调里面的fileList该与谁关联

---


# 获取select选中的label


```javascript

<template>
  <el-upload
    class="avatar-uploader"
    action="/wpk/file/upload"
    :show-file-list="false"
    :data="postData"
    name="upload_file"
    :before-upload="(res)=>{return beforeAvatarUpload(res,{width:512,height:512,size:1,type:'same'})}"
    :on-success="(res)=>{return handleAvatarSuccess(res,'icon')}">
    <img v-if="icon" :src="icon" class="avatar">
    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
  </el-upload>
  <p class="uploadtext">仅支持512*512像素，jpg或png格式，大小不超过1MB</p>
</template>

data:function(){
    return {      
        icon: ''
      }
    }
},
methods:{
    //上传前
    beforeAvatarUpload(file,json){               
        const isJPG = file.type === 'image/jpeg';
        const isPNG = file.type === 'image/png';
        const isLt2M = file.size / 1024 / 1024 < json.size;
     
        if (!isJPG && !isPNG) {
            this.$message.error('上传图片必须是JPG/PNG/格式!');
        }
        if (!isLt2M) {
            this.$message.error('上传图片大小不能超过 '+json.size+'M!');
        }

        //return (isJPG || isPNG) && isLt2M;

        let that = this;
        let canWidth = true;
        let canHeight = true;
        return new Promise(function(resolve, reject) {

            var reader = new FileReader();
            reader.readAsDataURL(file);
            reader.onload = function(event) {
                var image = new Image();
                image.src = event.target.result;
                image.onload = function () {
                    var width = this.width;
                    var height = this.height;

                    if(json.type == 'max'){                               
                        if( (width < json.width) || (height < json.width) || (width > json.height) || (height > json.height) ){
                            canWidth = false
                        }
                    }else{
                        canWidth = width == json.width
                        canHeight = height == json.height
                    }

                    if(!canWidth || !canHeight){
                        reject();
                        that.$message.error('请按照推荐尺寸上传');
                    }
                    resolve();
                };                 
            } 

        });                             
    },
    //上传成功后
    handleAvatarSuccess(res, type) {
        if(res.code!=0){
            this.$message.error(res.msg);
            return false;
        }
        if(type == 'icon'){
            this.icon = res.data.data.file_url;
        }
        if(type == 'bg'){
            this.artwork = res.data.data.file_url;
        }
    },   
}


```


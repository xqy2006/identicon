<template>
<div>
    <div class="markdown-body" style="margin-top: 15px;margin-inline-start: 15px;">
        <h1>GitHub头像生成器</h1>
        <strong>请输入用户名：</strong>
        <input v-model="username" class="form-control input" type="text" placeholder="用户名" aria-label="用户名" />
        <br />
        <strong>预览：</strong>
        <button class="btn btn-primary" @click="down()">下载</button>
        <br />
        <img :src="pic">
    </div>

</div>
</template>

<style lang="scss">
@import "@primer/css/index.scss";

html,
body {
    display: block;
    margin: 0px;
    margin-top: 0px;
    margin-right: 0px;
    margin-bottom: 0px;
    margin-left: 0px;
}
</style>

<script>
import Identicon from 'identicon.js';
import { md5 } from './md5.js';
export default {
    data() {
        return {
            username: "",
        };
    },
    computed: {
        pic:function(){
            var hash = md5(this.username);
            var data = new Identicon(hash, {
                size: 420,
                brightness: 0.71,
                saturation: 0.45
            }).toString();
            return 'data:image/png;base64,' + data;
        }
    },
    methods: {
        down() {
            const link = document.createElement('a');
            link.href = window.URL.createObjectURL(this.base64ToBlob(this.pic));
            link.download = "download.png";
            link.click();
        },
        base64ToBlob(dataURL, mimeType = null) {
            let arr = dataURL.split(','),
                defaultMimeType = arr[0].match(/:(.*?);/)[1],
                bstr = atob(arr[1]),
                n = bstr.length,
                u8arr = new Uint8Array(n);
            while (n--) {
                u8arr[n] = bstr.charCodeAt(n);
            }
            return new Blob([u8arr], {
                type: mimeType || defaultMimeType
            });
        },
    },
}
</script>

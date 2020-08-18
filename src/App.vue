<template>
    <div id="app">
        <div class="mdFileView">
            <div v-for="(item, index) in fileData" :key="index" class="fileItem" @click="_renderFile" :id="item.name">
                {{ item.name }}
            </div>
        </div>
        <div class="mdRenderView">
            <VueMarkdown replace="test" :source="mdData"></VueMarkdown>
        </div>
    </div>
</template>

<script>
import dataConfig from './data/dataConfig';
import axios from 'axios';
import VueMarkdown from 'vue-markdown';

export default {
    name: 'App',
    data() {
        return {
            fileData: [],
            mdData: null,
        };
    },
    components: {
        VueMarkdown,
    },
    mounted() {
        this._getFileItemData();
    },
    methods: {
        _getFileItemData() {
            this.fileData = dataConfig;
        },
        _renderFile(e) {
            const urlStr = `/files/${e.target.id}.md`;
            axios.get(urlStr).then((res) => {
                this.mdData = res.data;
            });
        },
    },
};
</script>

<style>
body {
    margin: 0 !important;
}
#app {
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-between;
}
.mdFileView {
    width: 300px;
    height: 100%;
}
.mdRenderView {
    width: calc(100% - 300px);
    height: 100%;
}
.fileItem {
    width: 100%;
    height: 40px;
    line-height: 40px;
    text-align: center;
    /* border-top: 1px solid #333333; */
    border-bottom: 1px solid #333333;
    cursor: pointer;
}
.fileItem:last-child {
    border-bottom: none;
}
.fileItem:hover {
    font-size: 20px;
    background-color: #333333;
    color: #ffffff;
}
</style>

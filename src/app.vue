<template>
    <div class="rec-files">
        <div class="header">
            <span class="title">{{i18n.RecentlyUsedFile}}</span>
            <span class="more" @click="toMore()">{{i18n.More}}</span>
        </div>
        <div class="content clearfloat" v-bind:style="{height: contentHeight}">
            <div class="item" v-for="(item, $index) in files" :key="item" v-bind:style="{height: rowHeight + 'px', width: pageWidth}">
                <div class="item_area">
                    <div class="file">
                        <img class="icon" @click="preview(item)" :src="item.iconPath" />
                        <span class="title" :title="item.name">{{item.name}}</span>
                    </div>
                    <span class="time" v-if='pageWidth'>{{item.modifiedTime}}</span>
                </div>
            </div>
            <div class="error-info" v-if="errMsg">
                <img src="static/styleImages/tea.svg" />
                <span @click="loadFiles()">{{errMsg}}</span>
            </div>
        </div>
    </div>
</template>

<script>
import apiSer from 'ser/api'

export default {
    data() {
        return {
            i18n: window.i18n,
            //限制显示行数
            limitRowsNum: 4,
            //每行高度
            rowHeight: 44,
            contentHeight: 'auto',
            files: [],
            errMsg: '',
            pageWidth: ''
        }
    },
    components: {
    },
    created() {
        this.contentHeight = this.limitRowsNum * this.rowHeight + 'px';
        this.loadFiles();
        app.linkplugin.listenRefreshWidgetData(() => {
            this.loadFiles();
        });
        this.resize()
    },
    mounted() {
    },
    methods: {
        resize() {
            var that = this
            window.onresize = function () {
                if (document.body.clientWidth > 400) {
                    that.pageWidth = document.body.clientWidth - 24 + 'px'
                }else {
                    that.pageWidth = ''
                }
            }
            window.onresize()
        },
        loadFiles() {
            apiSer.getRecentUsedFiles((files) => {
                this.files = files;
                this.errMsg = '';
            }, (errMsg) => {
                this.errMsg = errMsg;
            });
        },
        preview(row) {
            app.linkplugin.previewDiskFile({
                fileId: row.id,
                fileName: row.name,
                fileExt: row.extension,
                previewAvailable: row.preview.available,
                wopiSupport: row.preview.wopiSupport,
                canPreview: row.type == 'F'
            })
        },
        toMore() {
            app.linkplugin.jumpToDiskView(['RNODE_RECENTLYUSED']);
        }
    }
}
</script>

<style lang="scss">
@import "~asset/common";
@import "~asset/app";
</style>

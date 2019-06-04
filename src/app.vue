<template>
  <div class="rec-files">
    <div class="header">
      <span class="title">{{i18n.RecentlyUsedFile}}</span>
      <span class="more" @click="toMore()">{{i18n.More}}</span>
    </div>
    <div class="content clearfloat" v-bind:style="{height: contentHeight}">
      <div class="item" v-for="(item, $index) in files" :key="item">
        <div class="file">
          <img class="icon" @click="preview(item)" :src="item.iconPath" />
          <span class="title" :title="item.name">{{item.name}}</span>
        </div>
      </div>      
    </div>
  </div>
</template>

<script>
import apiSer from 'ser/api'

export default {
  data () {
    return {
      i18n: window.i18n,
      //限制显示行数
      limitRowsNum: 2,
      //每行高度
      rowHeight: 70,
      contentHeight: 'auto',
      files: []
    }
  },
  components: {
  },
  created(){
    apiSer.getRecentUsedFiles((files) => {
      this.files = files;
      this.contentHeight = this.limitRowsNum * this.rowHeight + 'px';
    }, (errMsg) => {
    });
  },
  mounted(){
  },
  methods: {
    preview(row){
      app.linkplugin.previewDiskFile({
        fileId: row.id,
        fileName: row.name,
        fileExt: row.extension,
        previewAvailable: row.preview.available,
        wopiSupport: row.preview.wopiSupport
      })
    },
    toMore(){
      app.linkplugin.jumpToDiskView();
    }
  }
}
</script>

<style lang="scss">
@import '~asset/common';
@import '~asset/app';
</style>

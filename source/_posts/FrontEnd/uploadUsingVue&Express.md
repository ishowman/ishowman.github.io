---
title: upload using Vue, Express and multer
date: 2019-01-21 20:01:15
tags: Vue Express practice
---

# 目的
在 Vue 中通过请求 express 的接口 api，实现图片上传功能。

# api 接口实现
使用 express & multer 来实现上传文件接口。
```
const express = require('express')
const router = express.Router()
const multer = require('multer')

const upload = multer({ dest: '/static/uploads' )

const multipleFileSymbol = 'files' // 对应前端 formData 字段名
const singleFileSymbol = 'file' // 对应前端 formData 字段名

// 多图上传接口
router.post('/files', upload.array(multipleFileSymbol, 12), (req, res, next) => {
  res.json({ files: req.files}) // 多图的信息放在 req.files上
})
// 单图上传接口
router.post('/file', upload.single(singleFileSymbol), (req, res, next) => {
  res.json({ file: req.file}) // 多图的信息放在 req.file上
})
```

# 前端 Vue 组件实现
```
<template>
  <div>
    <form enctype="multipart/form-data" @submit.prevent="uploadFile">
      <input ref="file" type="file" @change="handleImgChange()" />
      <button>单图上传</button>
    </form>

    <form enctype="multipart/form-data" @submit.prevent="uploadFiles">
      <input ref="files" type="file" multiple @change="handleFilesChange()" />

      <button>多图上传</button>
    </form>
  </div>
</template>
<script>
/* eslint-disable */
export default {
  data() {
    return {
      file: '',
      files: []
    }
  },
  methods: {
    handleImgChange() {
      this.file = this.$refs.file.files[0]
    },
    handleFilesChange () {
      this.files = this.$refs.files.files
    },
    async uploadFile() {
      const formData = new FormData()
      formData.append('file', this.file) // 这里的`file`字段必须对应接口里 multer 使用的字段
      const data = await this.$axios.post('/api/upload/file', formData)
    },
    async uploadFiles() {
      const formData = new FormData()
      for( let file of this.files) {
        formData.append('files', file) // 这里的`files`字段必须对应接口里 multer 使用的字段
      }
      const data = await this.$axios.post('/api/upload/files', formData)
    }
  }
}
</script>
```

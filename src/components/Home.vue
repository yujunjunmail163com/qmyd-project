<template>
  <div id="app">
    <div class="excel-upload">
      <label class="upload-label">
        <span>上传 Excel 文件</span>
        <input type="file" accept=".xlsx,.xls" @change="onExcelFileChange" class="upload-input" />
      </label>
      <p v-if="fileName" class="file-name">已选择：{{ fileName }}</p>
    </div>
  </div>
</template>

<script>
import * as XLSX from 'xlsx'

export default {
  name: 'App',
  components: {},
  data() {
    return {
      fileName: ''
    }
  },
  methods: {
    onExcelFileChange(event) {
      const file = event.target.files?.[0]
      if (!file) return

      this.fileName = file.name
      const reader = new FileReader()

      reader.onload = (e) => {
        try {
          const data = new Uint8Array(e.target.result)
          const workbook = XLSX.read(data, { type: 'array' })

          console.log('========== Excel 文档内容 ==========')
          console.log('工作簿名称:', workbook.Props?.Title || file.name)
          console.log('工作表列表:', workbook.SheetNames)

          workbook.SheetNames.forEach((sheetName) => {
            const sheet = workbook.Sheets[sheetName]
            const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1, defval: '' })
            console.log(`\n--- 工作表: ${sheetName} ---`)
            console.table(jsonData)
            console.log('原始数据（二维数组）:', jsonData)
          })

          console.log('========== 读取完成 ==========')
        } catch (err) {
          console.error('读取 Excel 失败:', err)
        }
      }

      reader.readAsArrayBuffer(file)
      event.target.value = ''
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.excel-upload {
  margin-top: 24px;
}

.upload-label {
  display: inline-block;
  padding: 10px 20px;
  background: #42b983;
  color: #fff;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
}

.upload-label:hover {
  background: #359268;
}

.upload-input {
  display: none;
}

.file-name {
  margin-top: 12px;
  font-size: 13px;
  color: #666;
}
</style>


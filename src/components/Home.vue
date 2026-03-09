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
      fileName: '',
      wirelessData: [] // 解析后的「无线通讯」数据（对象数组）
    }
  },
  methods: {
    /**
     * 解析「无线通讯」工作表：二维数组第 0 位为表头，其余为数据行
     * @param {Array[]} rawRows - sheet_to_json(header:1) 得到的二维数组
     * @returns {Object[]} 表头为 key 的对象数组
     */
    parseWirelessSheet(rawRows) {
      if (!rawRows || rawRows.length === 0) return []
      const headers = rawRows[0].map((h, i) => (h != null && String(h).trim() !== '' ? String(h).trim() : `列${i + 1}`))
      const dataRows = rawRows.slice(1)
      return dataRows.map((row) => {
        const item = {}
        headers.forEach((key, index) => {
          item[key] = row[index] != null ? row[index] : ''
        })
        return item
      })
    },

    onExcelFileChange(event) {
      const file = event.target.files?.[0]
      if (!file) return

      this.fileName = file.name
      const reader = new FileReader()

      reader.onload = (e) => {
        try {
          const data = new Uint8Array(e.target.result)
          const workbook = XLSX.read(data, { type: 'array' })

          const sheetName = '无线通信'
          if (!workbook.SheetNames.includes(sheetName)) {
            console.warn(`未找到工作表「${sheetName}」，当前工作表：`, workbook.SheetNames)
            this.wirelessData = []
            return
          }

          const sheet = workbook.Sheets[sheetName]
          const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1, defval: '' })

          // 第 0 位为表头，解析为对象数组
          const parsed = this.parseWirelessSheet(jsonData)
          this.wirelessData = parsed

          console.log('========== 「无线通讯」解析结果 ==========')
          // console.log('表头（第0行）:', jsonData[0])
          console.log('解析后数据（对象数组）:', JSON.parse(JSON.stringify(parsed)))
          // console.table(parsed)
          // console.log('========== 解析完成 ==========')
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


<template>
  <el-container>
    <el-header>
      <el-button type="text"
                 icon="el-icon-arrow-left">返回</el-button>
      <div class="title">
        <span>文件批次号：2023072500001</span>
        <span>AI算法版本号：1.0</span>
        <span>文件类型：逻辑图</span>
        <span>文件名称：文件1</span>
        <span>校核人：王凯</span>
      </div>
    </el-header>
    <el-main>
      <el-row :gutter="10">
        <el-col :span="16">
          <div class="layout-flex column">
            <div class="layout-flex-l">
              <of-fabric v-model="calloutList"
                         :image="demoPdf"
                         @choose="onChoose"></of-fabric>
            </div>
            <div class="layout-flex-s">
              <div class="filter justify-center bottom">
                <el-button size="small"
                           type="primary">上一页</el-button>
                <el-button size="small"
                           type="primary">下一页</el-button>
              </div>
            </div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="layout-flex column">
            <div class="layout-flex-s">
              <div class="filter justify-star top">
                <el-button size="small"
                           type="primary">新增</el-button>
                <el-button size="small"
                           type="primary">结果确认</el-button>
                <el-button size="small"
                           type="primary">返回</el-button>
              </div>
            </div>
            <div class="layout-flex-l">
              <el-table :data="tableData"
                        :header-cell-style="{'text-align':'center'}"
                        :cell-style="{'text-align':'center'}"
                        border
                        style="width: 100%"
                        height="100%">
                <el-table-column fixed
                                 type="selection">
                </el-table-column>
                <el-table-column type="index"
                                 label="序号"
                                 width="100">
                </el-table-column>
                <el-table-column prop="code"
                                 label="编码"
                                 width="100">
                </el-table-column>
                <el-table-column prop="codeAxis"
                                 label="编码坐标轴"
                                 width="150">
                </el-table-column>
                <el-table-column prop="confidence"
                                 label="置信度"
                                 width="100">
                </el-table-column>
                <el-table-column prop="source"
                                 label="数据来源"
                                 width="100">
                </el-table-column>
                <el-table-column prop="pageNumber"
                                 label="所在页码"
                                 width="100">
                </el-table-column>
                <el-table-column prop="isModify"
                                 label="是否修改"
                                 width="100">
                </el-table-column>
                <el-table-column prop="state"
                                 label="状态"
                                 width="100">
                </el-table-column>
                <el-table-column fixed="right"
                                 label="操作"
                                 width="150">
                  <template slot-scope="scope">
                    <el-button type="text"
                               size="small"
                               @click="onCheckClick(scope.row)">查看</el-button>
                    <el-button type="text"
                               size="small"
                               @click="onEditClick(scope.row)">编辑</el-button>
                  </template>
                </el-table-column>
              </el-table>
            </div>
            <div class="layout-flex-s">
              <div class="filter justify-end bottom">
                <el-pagination :current-page="1"
                               :page-sizes="[100, 200, 300, 400]"
                               :page-size="100"
                               layout=" prev, pager, next"
                               :total="400">
                </el-pagination>
              </div>
            </div>
          </div>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<script>
import OfFabric from '@/components/fabric'

export default {
  name: 'Demo',
  components: {
    OfFabric
  },
  data () {
    return {
      demoPdf: 'static/images/demo.jpg',
      title: '主件#PY2CI084021DSS043DD-1@1-A1-CAE-中文.pdf',
      calloutList: [
        [[700, 393], [974, 393], [974, 523], [700, 523]],
        [[1215, 521], [1532, 521], [1532, 638], [1215, 638]],
        [[1236, 965], [1396, 965], [1396, 1038], [1236, 1038]]
      ],
      tableData: [
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        },
        {
          code: '001',
          codeAxis: '10,20,20,10',
          confidence: '98%',
          source: 'AI结果',
          pageNumber: '1',
          isModify: '否',
          state: '已确认'
        }
      ]
    }
  },
  mounted () {
  },
  methods: {
    onChoose (target) {
      console.log(target)
      this.$notify({
        title: '选框坐标',
        message: target
      })
    },
    onCheckClick (row) {
      console.log(row)
    },
    onEditClick (row) {
      console.log(row)
    }
  }
}
</script>

<style scoped>
.el-container {
  width: 100vw;
  height: 100vh;
  padding: 5px;
}

.el-header {
  padding: 5px;
  border-bottom: dashed 1px #ccc;
  height: auto !important;
}

.el-header .title {
  display: inline-block;
  margin-left: 10px;
  font-size: 14px;
  line-height: 1;
  padding: 13px 0;
}

.el-main {
  padding: 5px;
}

.el-row,
.el-col {
  height: 100%;
}

.layout-flex {
  display: flex;
  flex: 1;
  flex-basis: auto;
  box-sizing: border-box;
  min-width: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.layout-flex.row {
  flex-direction: row;
}

.layout-flex.column {
  flex-direction: column;
}

.layout-flex.align-center {
  align-items: center;
}

.layout-flex.justify-center {
  justify-content: center;
}

.layout-flex > .layout-flex-s {
  flex-shrink: 0;
}

.layout-flex > .layout-flex-l {
  flex: 1;
  flex-basis: auto;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.layout-flex > .layout-flex-l.scroll-x {
  overflow-x: auto;
  overflow-y: hidden;
}

.layout-flex > .layout-flex-l.scroll-y {
  overflow-y: auto;
  overflow-x: hidden;
}

.layout-flex > .layout-flex-l.scroll {
  overflow: auto;
}

.filter {
  display: flex;
}

.filter.top {
  padding-bottom: 10px;
}

.filter.bottom {
  padding-top: 10px;
}

.filter.justify-center {
  justify-content: center;
}

.filter.justify-start {
  justify-content: start;
}

.filter.justify-end {
  justify-content: end;
}
</style>

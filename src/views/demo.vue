<template>
  <el-container>
    <el-header>
      <el-button type="text"
                 icon="el-icon-arrow-left">返回</el-button>
      <div class="title">{{ title }}</div>
    </el-header>
    <el-main>
      <of-fabric v-model="calloutList"
                 :image="demoPdf"></of-fabric>
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
      ]
    }
  },
  mounted () {
  },
  methods: {

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
</style>

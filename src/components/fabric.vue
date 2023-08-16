<template>
  <div class="of-fabric"
       :class="{'moving': isMoving, 'dragging': isDragging, 'editing': isEditing}">
    <div ref="refFabricWrapper"
         class="of-fabric__wrapper">
      <canvas id="canvas"
              ref="refFabricCanvas"
              class="of-fabric__canvas"></canvas>
    </div>
    <div class="of-fabric__toolbox">
      <el-button-group>
        <el-popover width="200"
                    trigger="hover"
                    placement="bottom-end">
          <div>滚动鼠标滑轮可缩放</div>
          <el-slider v-model="zoom"
                     :format-tooltip="percentage"
                     :step="0.1"
                     :min="0.01"
                     :max="2"
                     @input="onZoomSliderChange"></el-slider>
          <el-button size="small"
                     slot="reference">{{ percentage(zoom) }}</el-button>
        </el-popover>
      </el-button-group>
      <el-button-group>
        <el-tooltip class="item"
                    effect="dark"
                    content="图片比例 1:1"
                    placement="bottom">
          <el-button size="small"
                     @click="onSetZoomByMode(1)">1:1</el-button>
        </el-tooltip>
        <el-tooltip class="item"
                    effect="dark"
                    content="图片比例适配画布"
                    placement="bottom">
          <el-button size="small"
                     @click="onSetZoomByMode(2)">
            <i class="el-icon-refresh-right"></i>
          </el-button>
        </el-tooltip>
      </el-button-group>
      <el-radio-group v-model="ctrlType"
                      size="small"
                      @input="onCtrlTypeChange">
        <el-tooltip v-for="item in ctrlList"
                    :key="item.value"
                    class="item"
                    effect="dark"
                    :content="item.label"
                    placement="bottom">
          <el-radio-button :label="item.value">
            <i :class="item.icon"></i>
          </el-radio-button>
        </el-tooltip>
      </el-radio-group>
    </div>
  </div>
</template>

<script>
import { fabric } from 'fabric'

export default {
  name: 'OfFabric',
  props: {
    value: {
      type: Array,
      default: () => []
    },
    image: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      innerValue: [],
      canvas: null,
      zoom: 1,
      downPoint: null,
      isMoving: true,
      isDragging: false,
      isEditing: false,
      imageSize: {
        width: 1,
        height: 1
      },
      ctrlList: [
        {
          label: '移动画布(Alt)',
          icon: 'el-icon-rank',
          value: 'move'
        },
        {
          label: '编辑选框',
          icon: 'el-icon-edit-outline',
          value: 'drag'
        },
        {
          label: '新建选框',
          icon: 'el-icon-edit',
          value: 'draw'
        }
      ],
      ctrlType: 'move'
    }
  },
  watch: {
    value: {
      immediate: true,
      handler (n) {
        this.innerValue = n
      }
    }
  },
  mounted () {
    this.init()
    document.addEventListener('keydown', this.onKeyDown)
    document.addEventListener('keyup', this.onKeyUp)
  },
  methods: {
    init () {
      const _wrapper = this.$refs.refFabricWrapper
      const wrapperWidth = _wrapper.clientWidth
      const wrapperHeight = _wrapper.clientHeight

      this.canvas = new fabric.Canvas('canvas')

      this.canvas.setDimensions({
        width: wrapperWidth,
        height: wrapperHeight
      })

      // this.setBackgroundImage(this.image)
      // this.zoom = this.canvas.getZoom()

      this.imageSize.width = _wrapper.clientWidth
      this.imageSize.height = _wrapper.clientHeight
      fabric.Image.fromURL(this.image, img => {
        if (img) {
          this.imageSize.width = img.width
          this.imageSize.height = img.height

          this.canvas.setBackgroundImage(
            img,
            this.canvas.renderAll.bind(this.canvas)
          )
          this.zoom = Math.min(wrapperWidth / this.imageSize.width, wrapperHeight / this.imageSize.height)
          this.canvas.setZoom(this.zoom)
        }
      })

      if (this.innerValue.length > 0) {
        this.innerValue.forEach(item => {
          const polygonData = item.map(point => {
            return { x: point[0], y: point[1] }
          })
          const currentPolygon = new fabric.Polygon(
            polygonData,
            {
              fill: 'transparent',
              stroke: '#f00',
              strokeWidth: 2
            }
          )
          this.canvas.add(currentPolygon)

          // const rect = new fabric.Rect({
          //   top: item.top,
          //   left: item.left,
          //   width: item.width,
          //   height: item.height,
          //   fill: 'transparent',
          //   stroke: '#f00',
          //   strokeWidth: 2
          // })
          // this.canvas.add(rect)
        })
      }

      this.onCtrlTypeChange()

      this.canvas.on('mouse:wheel', this.onMouseWheel)
      this.canvas.on('mouse:down', this.onMouseDown)
      this.canvas.on('mouse:move', this.onMouseMove)
      this.canvas.on('mouse:up', this.onMouseUp)
    },
    percentage (val) {
      return Math.ceil(val * 100) + '%'
    },
    setBackgroundImage (url) {
      fabric.Image.fromURL(url, img => {
        if (img) {
          this.canvas.setBackgroundImage(
            img,
            this.canvas.renderAll.bind(this.canvas)
          )
        }
      })
    },
    onZoomSliderChange (val) {
      this.canvas.setZoom(val)
    },
    onSetZoomByMode (mode) {
      if (mode) {
        switch (mode) {
          case 1:
            this.zoom = 1
            break
          case 2:
            const _wrapper = this.$refs.refFabricWrapper
            const wrapperWidth = _wrapper.clientWidth
            const wrapperHeight = _wrapper.clientHeight
            this.zoom = Math.min(wrapperWidth / this.imageSize.width, wrapperHeight / this.imageSize.height)
            this.canvas.setZoom(this.zoom)
            break
          default:
            this.zoom = 1
            break
        }
      }
    },
    onCtrlTypeChange () {
      this.isMoving = this.ctrlType === 'move'
      this.isEditing = this.ctrlType === 'draw'

      if (this.ctrlType === 'move') {
        this.canvas.selection = false
        this.canvas.skipTargetFind = true // 禁止选中
      } else if (this.ctrlType === 'drag') {
        this.canvas.selection = true
        this.canvas.selectionColor = 'rgba(100, 100, 255, 0.3)'
        this.canvas.selectionBorderColor = 'rgba(255, 255, 255, 0.3)'
        this.canvas.skipTargetFind = false // 允许选中
      } else if (this.ctrlType === 'draw') {
        this.canvas.selection = true
        this.canvas.selectionColor = 'transparent'
        this.canvas.selectionBorderColor = 'rgba(255, 0, 0, 0.2)'
        this.canvas.skipTargetFind = true // 禁止选中
      }
    },
    onKeyDown (e) {
      const { keyCode } = e
      switch (keyCode) {
        case 18:
          this.isMoving = true
          break
        case 46:
          this.canvas.getActiveObjects().forEach(obj => {
            this.canvas.remove(obj)
          })
          break
      }
    },
    onKeyUp (e) {
      const { keyCode } = e
      switch (keyCode) {
        case 18:
          this.isMoving = false
          break
      }
    },
    onMouseWheel (opt) {
      let delta = opt.e.deltaY // 滚轮，向上滚一下是 -100，向下滚一下是 100
      this.zoom *= 0.999 ** delta
      if (this.zoom > 2) this.zoom = 2
      if (this.zoom < 0.01) this.zoom = 0.01
      this.canvas.zoomToPoint({ // 关键点
        x: opt.e.offsetX,
        y: opt.e.offsetY
      }, this.zoom)
      opt.e.preventDefault()
      opt.e.stopPropagation()
    },
    onMouseDown (opt) {
      let evt = opt.e
      if (this.ctrlType === 'move' || evt.altKey === true) { // 是否按住alt
        this.isDragging = true // isDragging 是自定义的
        this.canvas.lastPosX = evt.clientX // lastPosX 是自定义的
        this.canvas.lastPosY = evt.clientY // lastPosY 是自定义的
      } else if (this.ctrlType === 'draw') {
        this.downPoint = opt.absolutePointer
      }
    },
    onMouseMove (opt) {
      if (this.isDragging) {
        let evt = opt.e
        let vpt = this.canvas.viewportTransform // 聚焦视图的转换
        vpt[4] += evt.clientX - this.canvas.lastPosX
        vpt[5] += evt.clientY - this.canvas.lastPosY
        this.canvas.requestRenderAll()
        this.canvas.lastPosX = evt.clientX
        this.canvas.lastPosY = evt.clientY
      }
    },
    onMouseUp (opt) {
      let evt = opt.e
      this.canvas.setViewportTransform(this.canvas.viewportTransform) // 设置此画布实例的视口转换
      this.isDragging = false

      if (this.ctrlType === 'draw' && evt.altKey === false) {
        this.createPolygon(opt.absolutePointer)
      }

      this.$nextTick(() => {
        if (this.ctrlType !== 'move' && evt.altKey === false) {
          this.handlerComplete(this.canvas.getObjects())
        }
      })
    },
    handlerComplete (list) {
      const points = list && list.map(item => {
        const { aCoords } = item
        const { tl, tr, bl, br } = aCoords
        return [
          [tl.x, tl.y],
          [tr.x, tr.y],
          [br.x, br.y],
          [bl.x, bl.y]
        ]
      })
      this.$emit('input', points)
    },
    createRect (pointer) {
      // 点击事件，不生成矩形
      if (JSON.stringify(this.downPoint) === JSON.stringify(pointer)) {
        return
      }
      // 创建矩形
      // 矩形参数计算
      let top = Math.min(this.downPoint.y, pointer.y)
      let left = Math.min(this.downPoint.x, pointer.x)
      let width = Math.abs(this.downPoint.x - pointer.x)
      let height = Math.abs(this.downPoint.y - pointer.y)

      // 矩形对象
      const rect = new fabric.Rect({
        top,
        left,
        width,
        height,
        fill: 'transparent',
        stroke: '#f00',
        strokeWidth: 2
      })

      // 将矩形添加到画布上
      this.canvas.add(rect)

      this.downPoint = null
    },
    createPolygon (pointer) {
      if (JSON.stringify(this.downPoint) === JSON.stringify(pointer)) {
        return
      }
      const polygonData = [
        {
          x: Math.min(this.downPoint.x, pointer.x),
          y: Math.min(this.downPoint.y, pointer.y)
        },
        {
          x: Math.max(this.downPoint.x, pointer.x),
          y: Math.min(this.downPoint.y, pointer.y)
        },
        {
          x: Math.max(this.downPoint.x, pointer.x),
          y: Math.max(this.downPoint.y, pointer.y)
        },
        {
          x: Math.min(this.downPoint.x, pointer.x),
          y: Math.max(this.downPoint.y, pointer.y)
        }
      ]
      const currentPolygon = new fabric.Polygon(
        polygonData,
        {
          fill: 'transparent',
          stroke: '#f00',
          strokeWidth: 2
        }
      )
      this.canvas.add(currentPolygon)
    }
  }
}
</script>

<style scoped>
.of-fabric {
  position: relative;
  width: 100%;
  height: 100%;
}

.of-fabric.editing /deep/ .of-fabric__canvas {
  cursor: crosshair !important;
}

.of-fabric.moving /deep/ .of-fabric__canvas {
  cursor: grab !important;
}

.of-fabric.dragging /deep/ .of-fabric__canvas {
  cursor: grabbing !important;
}

.of-fabric__wrapper,
.of-fabric__canvas {
  width: 100%;
  height: 100%;
}

.of-fabric__toolbox {
  position: absolute;
  top: 5px;
  right: 5px;
  padding: 5px;
  border-radius: 4px;
  background: rgba(0, 0, 0, 0.5);
}
</style>

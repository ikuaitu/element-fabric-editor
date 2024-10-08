<template>
  <div class="box" v-if="mixinState.mSelectMode === 'one' && isImage">
    <el-divider content-position="left">
      <h4>图像描边</h4>
    </el-divider>
    <div class="hd-wrap">
      <div class="hd">
        <span>启用图像描边</span>
        <el-popover trigger="hover" content="只支持png透明图像">
          <template #reference>
            <el-icon color="#f34250"><WarningFilled /></el-icon>
          </template>
        </el-popover>
      </div>

      <el-switch
        v-model="openImgStroke"
        size="large"
        inline-prompt
        class="switch"
        active-text="开启"
        inactive-text="关闭"
        @change="onSwitchChange"
      />
    </div>

    <template v-if="openImgStroke">
      <div class="hd-wrap">
        <div class="hd">
          <span>是否只显示描边</span>
        </div>

        <el-switch
          inline-prompt
          v-model="isOnlyStroke"
          size="large"
          class="switch"
          active-text="是"
          inactive-text="否"
          @change="updateStroke"
        />
      </div>
      <div class="operation">
        <div class="hd" style="flex-basis: 98px">
          <span>描边大小</span>
        </div>
        <div style="width: 100%">
          <el-slider v-model="strokeWidth" :max="50" @change="onSliderChange" />
        </div>
      </div>

      <div class="operation" style="justify-content: space-between">
        <div class="hd">
          <span>描边颜色</span>
        </div>

        <div>
          <el-color-picker
            v-model="strokeColor"
            @change="onColorChange"
            placement="left"
          />
        </div>
      </div>
    </template>
  </div>
</template>

<script name="ImgStroke" lang="ts" setup>
import { WarningFilled } from '@element-plus/icons-vue'
import { fabric } from 'fabric'
import { Utils } from '@/lib/core'
import { useEditorStore } from '@/store/modules/editor'
import useSelect from '@/hooks/select'

const editorStore = useEditorStore()
interface IExtendImage {
  [x: string]: any
  originWidth?: number
  originHeight?: number
  originSrc?: string
}

const { mixinState } = useSelect()
const isImage = ref(false)
const openImgStroke = ref(false)
const strokeWidth = ref(5)
const strokeColor = ref('#000')
const isOnlyStroke = ref(false)
const getActiveObject = (): (fabric.Image & IExtendImage) | undefined => {
  const activeObject = editorStore.canvas?.getActiveObject()
  if (!activeObject || !Utils.isImage(activeObject)) return
  return activeObject
}

const setOrigin = () => {
  const _activeObject = getActiveObject()
  if (!_activeObject) return
  _activeObject.set('originWidth', _activeObject?.get('width'))
  _activeObject.set('originHeight', _activeObject?.get('height'))
  _activeObject.set('originSrc', _activeObject?.getSrc())
}

const updateStroke = () => {
  const strokeType = unref(isOnlyStroke) ? 'destination-out' : 'source-over'
  editorStore.editor.imageStrokeDraw(
    unref(strokeColor),
    unref(strokeWidth),
    strokeType
  )
}

const closeImgStroke = () => {
  strokeWidth.value = 0
  updateStroke()
}

const onSwitchChange = async (val: boolean) => {
  if (val) {
    unref(strokeWidth) === 0 && (strokeWidth.value = 5)
    setOrigin()
    updateStroke()
  } else {
    closeImgStroke()
  }
}

const onSliderChange = (val: number) => {
  strokeWidth.value = val
  updateStroke()
}

const onColorChange = (val: string) => {
  strokeColor.value = val
  updateStroke()
}

const handleSelectOne = () => {
  isImage.value = !!getActiveObject()
}

onMounted(() => {
  nextTick(() => {
    editorStore.editor?.on('selectOne', handleSelectOne)
  })
})

onBeforeUnmount(() => {
  editorStore.editor.off('selectOne', handleSelectOne)
})
</script>

<style lang="scss" scoped>
.box {
  margin-bottom: 20px;
  .hd-wrap {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
    .hd {
      flex: 1;
      & > span {
        margin-right: 5px;
      }
    }
  }
  .operation {
    display: flex;
    justify-content: space-between;
    align-items: center;
    .slide-wrap {
      width: 100%;
    }
  }
}
</style>

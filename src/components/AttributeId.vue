<!--
 * @Author: June
 * @Description: 
 * @Date: 2024-09-05 23:00:59
 * @LastEditTime: 2024-09-06 11:18:24
 * @LastEditors: June
 * @FilePath: \ai-desing\src\views\editor\components\AttributeId.vue
-->
<template>
  <div class="mb-10px attr-item-box" v-if="mixinState.mSelectMode === 'one'">
    <!-- <h3>数据</h3> -->
    <el-divider content-position="left"><h4>数据</h4></el-divider>

    <el-form :label-width="40" class="form-wrap">
      <el-form-item :label="$t('editor.attributes.id')">
        <el-input
          v-model="baseAttr.id"
          @change="changeCommon('id', baseAttr.id)"
          size="small"
        ></el-input>
      </el-form-item>
    </el-form>

    <el-row :gutter="10">
      <el-col :span="12">
        <el-select
          v-model="baseAttr.linkData[0]"
          filterable
          allow-create
          @change="changeCommon('linkData', baseAttr.linkData)"
        >
          <el-option value="src"></el-option>
          <el-option value="text"></el-option>
        </el-select>
      </el-col>
      <el-col :span="12">
        <el-input v-model="baseAttr.linkData[1]" placeholder="请输入" />
      </el-col>
    </el-row>
  </div>
</template>

<script lang="ts" setup>
import { Selector } from '@/hooks/useSelectListen'
import { useEditorStore } from '@/store/modules/editor'

const mixinState = inject('mixinState') as Selector
const editorStore = useEditorStore()

const update = getCurrentInstance()

// 属性值
const baseAttr = reactive({
  id: 0,
  linkData: ['', '']
})

// 属性获取
const getObjectAttr = (e?: any) => {
  const activeObject: any = editorStore.canvas?.getActiveObject()
  // 不是当前obj，跳过
  if (e && e.target && e.target !== activeObject) return
  if (activeObject) {
    baseAttr.id = activeObject.get('id')
    baseAttr.linkData = activeObject.get('linkData') || ['', '']
  }
}

// 通用属性改变
const changeCommon = (key: any, value: any) => {
  const activeObject = editorStore.canvas?.getActiveObjects()[0]
  if (activeObject) {
    activeObject && activeObject.set(key, value)
    editorStore.canvas?.renderAll()
  }
}

const selectCancel = () => {
  update?.proxy?.$forceUpdate()
}

onMounted(() => {
  nextTick(() => {
    // 获取字体数据
    getObjectAttr()
    editorStore.editor?.on('selectCancel', selectCancel)
    editorStore.editor?.on('selectOne', getObjectAttr)
    editorStore.canvas?.on('object:modified', getObjectAttr)
  })
})

onBeforeUnmount(() => {
  editorStore.editor?.off('selectCancel', selectCancel)
  editorStore.editor?.off('selectOne', getObjectAttr)
  editorStore.canvas?.off('object:modified', getObjectAttr)
})
</script>

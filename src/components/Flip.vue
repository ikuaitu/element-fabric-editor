<!--
 * @Author: June
 * @Description: 
 * @Date: 2024-09-05 23:16:20
 * @LastEditTime: 2024-09-06 11:37:53
 * @LastEditors: June
 * @FilePath: \ai-desing\src\views\editor\components\Flip.vue
-->
<template>
  <div v-if="mixinState.mSelectMode === 'one'" class="attr-item-box">
    <div class="bg-item">
      <el-tooltip :content="$t('editor.attrSetting.flip.x')">
        <el-button :disabled="notSelectOneMode()" @click="flip('X')" link>
          <svg
            t="1650443094178"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="1549"
            width="14"
            height="14"
          >
            <path
              d="M252.76928 299.904l146.2784 0 0 472.42752-146.2784 0 0-472.42752Z"
              p-id="1550"
            ></path>
            <path
              d="M477.48096 85.34528l70.87104 0 0 885.80608-70.87104 0 0-885.80608Z"
              p-id="1551"
            ></path>
            <path
              d="M629.80096 284.8l31.0016 0 0 502.88128-31.0016 0L629.80096 284.8zM776.42752 284.8l31.0016 0 0 502.88128-31.0016 0L776.42752 284.8zM657.09056 315.8016l0-31.0016 123.04896 0 0 31.0016L657.09056 315.8016zM657.27488 787.64544l0-31.0016 123.04896 0 0 31.0016L657.27488 787.64544z"
              p-id="1552"
            ></path>
          </svg>
        </el-button>
      </el-tooltip>
      <el-tooltip :content="$t('editor.attrSetting.flip.y')">
        <el-button :disabled="notSelectOneMode()" @click="flip('Y')" link>
          <svg
            t="1650443104385"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="1749"
            width="14"
            height="14"
          >
            <path
              d="M286.01856 250.91584l472.4224 0 0 146.2784-472.4224 0 0-146.2784Z"
              p-id="1750"
            ></path>
            <path
              d="M87.19872 475.62752l885.80096 0 0 70.87104-885.80096 0 0-70.87104Z"
              p-id="1751"
            ></path>
            <path
              d="M773.55008 627.94752l0 31.0016L270.6688 658.94912l0-31.0016L773.55008 627.94752zM773.55008 774.5792l0 31.0016L270.6688 805.5808l0-31.0016L773.55008 774.5792zM742.54848 655.24224l31.0016 0 0 123.04896-31.0016 0L742.54848 655.24224zM270.70464 655.42144l31.0016 0 0 123.04896-31.0016 0L270.70464 655.42144z"
              p-id="1752"
            ></path>
          </svg>
        </el-button>
      </el-tooltip>
    </div>

    <!-- <Divider plain></Divider> -->
  </div>
</template>

<script lang="ts" setup>
import { Selector } from '@/hooks/useSelectListen'
import { useEditorStore } from '@/store/modules/editor'

const mixinState = inject('mixinState') as Selector
const editorStore = useEditorStore()

// 非单选时，禁止镜像操作
const notSelectOneMode = () => {
  return mixinState.mSelectMode !== 'one'
}
const flip = (type: string) => {
  const activeObject: any = editorStore.canvas?.getActiveObject()
  activeObject.set(`flip${type}`, !activeObject[`flip${type}`]).setCoords()
  editorStore.canvas?.requestRenderAll()
}
</script>

<style scoped lang="scss">
:deep(.el-button) {
  flex: 1;
  &[disabled] {
    svg {
      opacity: 0.2;
    }
  }
}

.attr-item-box {
  margin-top: 8px;
}
</style>

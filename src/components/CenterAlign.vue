<template>
  <div class="attr-item-box" v-if="mixinState.mSelectMode">
    <el-divider content-position="left">
      <h4>{{ $t('editor.attrSetting.centerAlign.name') }}</h4>
    </el-divider>
    <div class="bg-item">
      <!-- 水平集中 -->
      <el-tooltip :content="$t('editor.attrSetting.centerAlign.centerX')">
        <el-button
          :disabled="!mixinState.mSelectMode"
          @click="position('centerH')"
          link
        >
          <svg
            t="1650442559691"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="3787"
            width="14"
            height="14"
          >
            <path
              d="M885 607H544V417h192V227H544V24h-64v203H288v190h192v190H139v190h341v203h64V797h341z"
              p-id="3788"
            ></path>
          </svg>
        </el-button>
      </el-tooltip>
      <!-- 水平垂直居中 -->
      <el-tooltip :content="$t('editor.attrSetting.centerAlign.center')">
        <el-button
          :disabled="!mixinState.mSelectMode"
          @click="position('center')"
          link
        >
          <svg
            t="1650852784867"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="2351"
            width="14"
            height="14"
          >
            <path
              d="M544 480V64h-64v416H64v64h416v416h64V544h416v-64z"
              p-id="2352"
            ></path>
            <path
              d="M123.7 241.1h119.5v64H123.7zM302.9 241.1h119.5v64H302.9zM601.6 241.1h119.5v64H601.6zM780.8 241.1h119.5v64H780.8zM123.7 718.9h119.5v64H123.7zM302.9 718.9h119.5v64H302.9zM601.6 718.9h119.5v64H601.6zM780.8 718.9h119.5v64H780.8z"
              p-id="2353"
            ></path>
          </svg>
        </el-button>
      </el-tooltip>
      <!-- 垂直居中 -->
      <el-tooltip :content="$t('editor.attrSetting.centerAlign.centerY')">
        <el-button
          :disabled="!mixinState.mSelectMode"
          @click="position('centerV')"
          link
        >
          <svg
            t="1650442510967"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="3412"
            width="14"
            height="14"
          >
            <path
              d="M859.9 474H164.1c-4.5 0-8.1 3.6-8.1 8v60c0 4.4 3.6 8 8.1 8h695.8c4.5 0 8.1-3.6 8.1-8v-60c0-4.4-3.6-8-8.1-8z m-353.6-74.7c2.9 3.7 8.5 3.7 11.3 0l100.8-127.5c3.7-4.7 0.4-11.7-5.7-11.7H550V104c0-4.4-3.6-8-8-8h-60c-4.4 0-8 3.6-8 8v156h-62.8c-6 0-9.4 7-5.7 11.7l100.8 127.6z m11.4 225.4c-2.9-3.7-8.5-3.7-11.3 0L405.6 752.3c-3.7 4.7-0.4 11.7 5.7 11.7H474v156c0 4.4 3.6 8 8 8h60c4.4 0 8-3.6 8-8V764h62.8c6 0 9.4-7 5.7-11.7L517.7 624.7z"
              p-id="3413"
            ></path>
          </svg>
        </el-button>
      </el-tooltip>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { Selector } from '@/hooks/useSelectListen'
import { useEditorStore } from '@/store/modules/editor'

const mixinState = inject('mixinState') as Selector
const editorStore = useEditorStore()
const position = (name: string) => {
  editorStore.editor.position(name)
}
</script>
<style scoped lang="scss">
:deep(.el-button) {
  &[disabled] {
    svg {
      opacity: 0.2;
    }
  }
}
svg {
  vertical-align: text-bottom;
}
</style>

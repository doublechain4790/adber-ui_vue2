<template>
  <div class="flex-column-page-wrap pageWrap">
    <AdTabs
      v-model="tabs_list"
      :activeTabId.sync="tabs_activeId"
      :loading='tabs_loading'
      @switch="tabs_switchTab"
      @sort="tabs_tabSort"
      @edit="tabs_tabEdit"
      @delete="tabs_tabDelete"
    />

    <AdTable
      class="local_table tabs_content-wrap"
      :searchParams.sync="searchParams"
      :list="localList"
      :total="total"
      :options="options"
      :columns="localColumns"
      :checkedOptions.sync="tabs_checkedColumns"
      :columnsConfig="tabs_columnsConfig"
      @sortChange='testSortChange'
      @update:checkedOptions="tabs_updateCheckedColumns"
    >
      <template #toolLeft>
<!--        <AdSearchGroup/>-->
        <ad-search-group
          class="local_search-group-wrap"
          :forms="tabs_filterForms"
          v-model="tabs_filterParams"
          :loading='tabs_loading'
          :selectedSettingSubmit="tabs_selectedSettingSubmit"
          :tabCreateSubmit="tabs_tabCreate"
          :deleteTag="deleteTag">
          <template #prepend>
            <el-input
              class="ad-input-search"
              v-model="tabs_filterParams.pattern"
              size="medium"
              :placeholder="$t('outboundOrder.filter.pattern.placeholder')"
              clearable
              @keyup.enter.native="updateParams"
            >
              <i slot="suffix" class="el-input__icon el-icon-search" @click="updateParams"/>
            </el-input>
          </template>
          <template #adSelectSlot="{option, label}">
            <div style="background: #A0AAB7;">{{label}} + {{ option.value_1 }}</div>
          </template>
        </ad-search-group>
      </template>
      <template #top>
        <!-- 按钮集合 -->
        <div class="ad-button-wrap">
          <template v-if="true">
            <el-tooltip
              placement="top"
              :content="$t('outboundOrder.btn.CreateBatchTip')"
            >
              <el-button
                type="primary"
                :class="{ 'is-disabled_test': true }"
              >
                {{ $t('outboundOrder.btn.CreateBatch') }}
              </el-button>
            </el-tooltip>

            <el-button
              style="margin-right: 10px"
            >
              {{ $t('outboundOrder.btn.BulkPicking') }}
            </el-button>
          </template>
<!--          :disabled="!selectList.length"-->
          <el-dropdown>
            <el-button>
              {{ $t('outboundOrder.btn.Print') }}
              <i class="el-icon-arrow-down el-icon--right"></i>
            </el-button>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item v-for="v of dropdownList" :key="v" :command="v">
                {{ $t(`outboundOrder.btn.${v}`) }}
              </el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
      </template>
      <!-- 自定义slot title -->
      <template #slotName_title>
        <div class="slot_title-wrap">
          <el-tooltip placement="top" content="slotName_title by localUser">
            <span class='label'>slotName_title by localUser</span>
          </el-tooltip>
        </div>
      </template>
      <!-- 操作 -->
      <template #action="{ row }">
        <el-tooltip placement="top" :content="$t('outboundOrder.btn.view')">
          <el-button class="ad-icon-button">
            <AdIcon icon-class="ad-view"></AdIcon>
          </el-button>
        </el-tooltip>
        <el-button class="ad-icon-button">
          <AdIcon icon-class="ad-hide_column"></AdIcon>
        </el-button>
        <el-button class="ad-icon-button" disabled>
          <AdIcon icon-class="ad-hide_column"></AdIcon>
        </el-button>
      </template>
    </AdTable>
  </div>
</template>

<script>
import {
  // columns,
  tableBaseMixin
} from './default_config'

export default {
  name: 'default',
  mixins: [tableBaseMixin],
  beforeCreate() {
    console.error('before create default ElTable')
    // columns.find(v => v.prop === 'local_user').slots.default = (scope, h) => {
    //   return [<div style={'background: #f0f;'}>slot_user 函数渲染 slot:default </div>]
    // }
  },
  created() {
    // this.tabs_key = 'ui_tableDefault_0'
  }
}
</script>

<style scoped lang="scss">
.pageWrap {
  //padding-top: 12px;
  padding: 12px 12px 0 12px;
  overflow: auto;
  background-color: #f5f6f7;
}
// 其他样式
.local_table {
  //padding: 0 12px;
  box-shadow: 0 0 6px 4px rgb(145 159 175 / 6%);
  border-top: 1px solid #eaedf0;
  border-radius: 6px 6px 0 0;
  background-color: #fff;
  padding: 16px 16px 0 16px;
  &.tabs_content-wrap {
    border-top: 0;
    border-radius: 0;
  }
  // 若因调整 toolRight 导致宽度 不为96px 时 需要做调整 调整1 eg:
  /*&::v-deep {
    .ad-column-wrap {
      display: none;
    }
  }*/
}
.local_search-group-wrap ::v-deep {
  // 若因调整 toolRight 导致宽度 不为96px 时 需要做调整 调整2 eg:
  /*.tags-wrap {
    margin-right: 48px;
  }
  .ad-search-group-tags .tags-action {
    right: -48px;
  }*/
}

</style>

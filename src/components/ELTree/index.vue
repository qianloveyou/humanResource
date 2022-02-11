<template>
  <div class="elTreeCss">
    <el-input
      v-model="filterText"
      :placeholder="$t('vTree.filterPlaceholder')"
      size="small"
      v-if="needFilterInput"
    />
    <el-scrollbar wrap-style="overflow-x:hidden;">
      <el-tree
        ref="tree"
        class="filter-tree"
        default-expand-all
        :data="treeData"
        :props="defaultProps"
        :filter-node-method="filterNode"
        :render-after-expand="renderAfterExpand"
        :expand-on-click-node="expandOnClickNode"
      >
        <span class="custom-tree-node" slot-scope="{ node, data }" v-if="needCustomNode">
          <slot name="treeNode" :nodeData="{node, data}" />
        </span>
        <span class="el-tree-node__label" slot-scope="{ node }" v-else>{{node.label}}</span>
      </el-tree>
    </el-scrollbar>
  </div>
</template>
<script>
export default {
  props: {
    treeData: {
      type: Array,
      default: () => ([]),
    },
    defaultProps: {
      type: Object,
      default: () => ({
        children: 'children',
        label: 'label',
      }),
    },
    // 是否需要过滤条件
    needFilterInput: {
      type: Boolean,
      default: () => false,
    },
    // 是否需要自定义节点
    needCustomNode: {
      type: Boolean,
      default: () => false,
    },
    // 是否在第一次展开某个树节点后才渲染其子节点
    renderAfterExpand: {
      type: Boolean,
      default: () => true,
    },
    // 是否在点击节点的时候展开或者收缩节点， 默认值为 true，如果为 false，则只有点箭头图标的时候才会展开或者收缩节点。
    expandOnClickNode: {
      type: Boolean,
      default: () => true,
    },
  },
  data () {
    return {
      filterText: '',
    }
  },
  watch: {
    filterText (newVal, oldVal) {
      this.$refs.tree.filter(newVal)
    },
  },
  methods: {
    filterNode (value, data) {
      if (!value) {
        return true
      }
      return data.label.indexOf(value) !== -1
    },
  },
}
</script>
<style lang="less" scoped>
.el-scrollbar {
  height: calc(100% - 12px);
}
</style>

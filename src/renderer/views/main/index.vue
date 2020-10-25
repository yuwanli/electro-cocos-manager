<template>
  <div id="wrapper">
    <el-tree
      :data="data"
      :props="defaultProps"
      @node-contextmenu="contextmenu"
      @node-click="handleNodeClick"
      @node-expand="expand"
      @node-collapse="collapse"
    >
      <template slot-scope="item">
        <p class="item">
          <i
            :class="[
              'icon',
              item.data.children
                ? item.data.isOpen
                  ? 'el-icon-folder-opened'
                  : 'el-icon-folder'
                : 'el-icon-document',
            ]"
          ></i
          >{{ item.data.label }}
        </p>
      </template>
    </el-tree>
  </div>
</template>

<script>
const fs = require('fs');
const join = require('path').join;
const basePath = '/Users/11111282/vivo/code/minigame/landlord';
import './index.less';
export default {
  name: 'main',
  components: {},
  data() {
    return {
      data: [
        {
          label: '一级 1',
          children: [
            {
              label: '二级 1-1',
              children: [
                {
                  label: '三级 1-1-1',
                },
              ],
            },
          ],
        },
        {
          label: '一级 2',
          children: [
            {
              label: '二级 2-1',
              children: [
                {
                  label: '三级 2-1-1',
                },
              ],
            },
            {
              label: '二级 2-2',
              children: [
                {
                  label: '三级 2-2-1',
                },
              ],
            },
          ],
        },
        {
          label: '一级 3',
          children: [
            {
              label: '二级 3-1',
              children: [
                {
                  label: '三级 3-1-1',
                },
              ],
            },
            {
              label: '二级 3-2',
              children: [
                {
                  label: '三级 3-2-1',
                },
              ],
            },
          ],
        },
      ],
      defaultProps: {
        children: 'children',
        label: 'label',
      },
    };
  },
  created() {
    this.data = this.getFolderList(basePath);
    console.log(this.data);
  },
  computed: {},
  methods: {
    expand(data) {
      data.isOpen = true;
      console.log(this.data);
    },
    collapse(data) {
      data.isOpen = false;
    },
    getFolderList(path) {
      const root = fs.readdirSync(path);
      return root.reduce((res, item) => {
        if (/(^\.\w+)|(node_modules)|(\.meta)/.test(item)) {
          return res;
        }
        const obj = {};
        obj.label = item;
        obj.isOpen = false;
        let fPath = join(path, item);
        let stat = fs.statSync(fPath);
        if (stat.isDirectory()) {
          obj.children = this.getFolderList(fPath);
        }
        res.push(obj);
        return res;
      }, []);
    },
    contextmenu(data) {
      console.log(data);
    },
    handleNodeClick(data) {
      console.log(data);
    },
  },
};
</script>

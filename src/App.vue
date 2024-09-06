<template>
  <div style="height: 500px;">
    <VTree :data="data" animation>

      <template v-slot:node="{ node }">
        <span class="custom-tree-node-2">
          {{ node.name }}
        </span>
      </template>

    </VTree>
  </div>
</template>

<script>
import VTree from '@wsfe/ctree'
import treeData from './org.js'

function buildTree(data, root = '') {
    const map = {}
    const result = []

    for (let i = 0; i < data.length; i++) {
        const item = data[i]
        if (!map[item.id]) {
            map[item.id] = item
        }
    }

    for (let k = 0; k < data.length; k++) {
        const item = data[k]
        if (map[item.pid] && map[item.pid] !== root) {

            map[item.pid].children = map[item.pid].children || []
            map[item.pid].children.push(item)
        } else {
            result.push(map[item.id])
        }
    }

    return result
}

export default {
  name: 'App',
  components: {
    VTree
  },
  data() {
    return {
      data: []
    }
  },
  mounted() {
    this.data = Object.freeze(buildTree(treeData))
  }
}
</script>

<style>
@import '~@wsfe/ctree/dist/ctree.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<template>
  <div style="display: flex">
    <div class="group"></div>
    <div>
      <el-tree
        style="margin-right: 200px"
        :data="data"
        show-checkbox
        node-key="id"
        default-expand-all
        @node-drag-start="handleDragStart"
        @node-drag-enter="handleDragEnter"
        @node-drag-leave="handleDragLeave"
        @node-drag-over="handleDragOver"
        @node-drag-end="handleDragEnd"
        @node-drop="handleDrop"
        :draggable="true"
        
        :allow-drop="allowDrop"
        :allow-drag="allowDrag"
      >
      </el-tree>
    </div>
    <div
      id="drop"
      @drop="drop($event)"
      @dragover="dropOver($event)"
      class="drop-block"
    ></div>
  </div>
</template>

<script>
import Editor from "wangeditor";
export default {
  name: "Home",
  data() {
    return {
      draggable: true,
      tableText: "",
      list: [
        {
          item: "1",
          id: "1",
        },
        {
          item: "2",
          id: "2",
        },
      ],
      data: [
        {
          id: 1,
          label: "一级 1",
          children: [
            {
              id: 4,
              label: "二级 1-1",
              children: [
                {
                  id: 9,
                  label: "三级 1-1-1",
                },
                {
                  id: 10,
                  label: "???",
                },
              ],
            },
          ],
        },
        {
          id: 2,
          label: "一级 2",
          children: [
            {
              id: 5,
              label: "二级 2-1",
            },
            {
              id: 6,
              label: "二级 2-2",
            },
          ],
        },
        {
          id: 3,
          label: "一级 3",
          children: [
            {
              id: 7,
              label: "二级 3-1",
            },
            {
              id: 8,
              label: "二级 3-2",
            },
          ],
        },
      ],
      defaultProps: {
        children: "children",
        label: "label",
      },
    };
  },
  mounted() {
    var editor = new Editor("#drop");
    editor.customConfig.onchange = (html) => {
      console.log(editor.txt.html());
    };
  
    editor.create();
  },
  methods: {
    dropOver(e) {
      e.preventDefault(); // 在拖动中阻止默认事件
    },

    drop(e) {
      console.log(e);
      e.preventDefault();
      // 将拖动元素旋转到目标区域中
      // var data = e.dataTransfer.getData("Text");
      // console.log(data);
      e.target.innerHTML = this.tableText;
    },

    handleDragStart(node, ev) {
      // console.log("drag start", node);
    },
    handleDragEnter(draggingNode, dropNode, ev) {
      // console.log("tree drag enter: ", dropNode.label);
    },
    handleDragLeave(draggingNode, dropNode, ev) {
      // console.log("tree drag leave: ", dropNode.label);
    },
    handleDragOver(draggingNode, dropNode, ev) {
      console.log("tree drag over: ", dropNode.label);
      // this.drop(dropNode.label)
      this.tableText = dropNode.label;
    },
    handleDragEnd(draggingNode, dropNode, dropType, ev) {
      // console.log("tree drag end: ", dropNode && dropNode.label, dropType);
    },
    handleDrop(draggingNode, dropNode, dropType, ev) {
      console.log("tree drop: ", dropNode.label, dropType);
    },

    allowDrop(draggingNode, dropNode, type) {
      if (dropNode.data.label === "二级 3-1") {
        return type !== "inner";
      } else {
        return true;
      }
    },
    allowDrag(draggingNode) {
      return draggingNode.data.label.indexOf("三级 3-2-2") === -1;
    },
  },
};
</script>
<style lang="scss">
.group {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  width: 500px;
  height: 200px;
}

.group > div {
  margin: 20px;
  border: 1px solid #333;
  width: 200px;
  cursor: pointer;
}

.group > div p {
  background: red;
}
</style>

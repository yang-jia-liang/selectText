<template>
  <h1 >Diginotes</h1>

    <el-button type="primary" :disabled="!isEdit" @click="addCircle">圆圈</el-button>
    <el-button type="primary" :disabled="!isEdit" @click="addHighlight">高亮</el-button>
    <el-dropdown @visible-change="visibleChange" @command="addDelete">
        <el-button type="primary" :disabled="!isEdit" >删除</el-button>
        <template #dropdown>
            <el-dropdown-menu>
                <el-dropdown-item command=""><del>横向删除</del></el-dropdown-item>
                <el-dropdown-item command="tilt"><del class="tilt">斜向删除</del></el-dropdown-item>
            </el-dropdown-menu>
        </template>
    </el-dropdown>


    <el-tooltip content="谷歌不支持选中多组文字，火狐可以" placement="top">
        <el-dropdown @command="handleMarkTags">
            <el-button type="primary">选中所有</el-button>
            <template #dropdown>
                <el-dropdown-menu>
                    <el-dropdown-item command=".circle">圆圈</el-dropdown-item>
                    <el-dropdown-item command=".highlight">高亮</el-dropdown-item>
                    <el-dropdown-item command="del">删除</el-dropdown-item>
                </el-dropdown-menu>
            </template>
        </el-dropdown>
    </el-tooltip>



    <p ref="p" @click="handleSelect">whoever a veterinarian chief of pediatric surgery fully relatively frequently Calliope Dr.
    Cristina Yang Richard orthopedic surgeon Mark Finn Olivia seriouslyApril highly most Tyler cried quite
    <span class="circle"> <del>Dr. Sydney Heron push </del> </span>
    one of epi begin Dr. Sydney Heron surgical resident totally Dr. Richard Webber start a central line essentially
    relatively surgicial definitely apparently effectively
    <span class="highlight">Dr. Cristina Yang an intern a nurse Yang
      <span class="circle">orthopedic</span>
      surgeon okay Dr. Jackson Avery practically
    </span>
    Alex approximately Isobel Meredith's father Jackson obviously a surgical resident really
    </p>
</template>

<script setup>
  import { reactive, toRefs } from 'vue'

  const state = reactive({
      p: null,
      input: null,
      isEdit: false,         // 编辑按钮是否可用
      tempSelectRange: null, // 存放selection.range，避免点击dropDown元素后焦点丢失，取不到原来的selection.range
  });
  let { isEdit, tempSelectRange } = toRefs(state);

  const visibleChange = (visible) => {
      tempSelectRange =  visible ? window.getSelection().getRangeAt(0) : null;
  };

  const handleSelect = () => {
    const selectText = window.getSelection().toString();

    isEdit.value = selectText ? true : false;
  };

  const handleMarkTags = (selector) => {
      const tags = document.querySelectorAll(`${selector}`);
      const selection = window.getSelection();

      if(selection.rangeCount > 0) {
          selection.removeAllRanges();
      }

      for (let i = 0; i < tags.length; i++) {
          const range = document.createRange();
          range.selectNode(tags[i]);
          selection.addRange(range);
      }
  };

  const addCircle = () => {
    const selectRange = window.getSelection().getRangeAt(0);
    const circleTag = document.createElement("span");
    circleTag.className = 'circle';

    selectRange.surroundContents(circleTag);
  };

  const addDelete = (direction) => {
      const selectRange = tempSelectRange;
      const delTag = document.createElement("del");

      if (direction === 'tilt') {
          delTag.className += 'tilt';
      }

      selectRange.surroundContents(delTag);
  };

  const addHighlight = () => {
    const selectRange = window.getSelection().getRangeAt(0);
    const highlightTag = document.createElement("span");
    highlightTag.className = 'highlight';

    selectRange.surroundContents(highlightTag);
  };
</script>

<style lang="less">
  body {
    color: #2a2135;
    background-color: #fbf4ff;
    font-family: "Caveat Brush", cursive;
  }

  p {
    line-height: 30px;
    font-size: 24px;
    max-width: 800px;
  }

  .highlight {
    background-color: #c2e9fb;
    background-image: linear-gradient(to right, #a1c4fd 0%, #c2e9fb 100%);
  }

  .circle {
    border-style: solid;
    border-color: #b71919;
    border-radius: 50%;
    border-width: medium thin thick 10px;
  }

  del {
        background-color: #b719192b;
        color: #a12323;

      &.tilt {
          position: relative;
          text-decoration: none;

          &::before {
              content: "";
              position: absolute;
              left: 0;
              top: 50%;
              right: 0;
              border-top: 2px solid;
              transform: rotate(8deg);
          }
      }
  }

  // 选中状态颜色
  ::-moz-selection,
  ::selection {
    background-color: #d7a7cd;
    background-image: linear-gradient(to right, #b285bf 0%, #d7a7cd 100%);
  }
</style>
<style lang="less" scoped>
    .el-button {
        border-radius: 0;
        border-right: 1px solid #fff;
    }
    .el-button + .el-button {
        margin-left: 0;
    }

    .el-dropdown {
        margin-right: 12px;
    }
</style>

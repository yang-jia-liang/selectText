<template>
  <h1 >Diginotes</h1>

  <el-button-group>
    <el-button :disabled="!isEdit" :icon="Cloudy" @click="addCircle" />
    <el-button :disabled="!isEdit" :icon="Close" @click="addDelete" />
    <el-button :disabled="!isEdit" :icon="BrushFilled" @click="addHighlight" />
  </el-button-group>

  <p @click="handleSelect">whoever a veterinarian chief of pediatric surgery fully relatively frequently Calliope Dr.
    Cristina Yang Richard orthopedic surgeon Mark Finn Olivia seriouslyApril highly most Tyler cried quite
    <span class="error"> <del>Dr. Sydney Heron push </del> </span>
    one of epi begin Dr. Sydney Heron surgical resident totally Dr. Richard Webber start a central line essentially
    relatively surgicial definitely apparently effectively
    <span class="highlight">Dr. Cristina Yang an intern a nurse Yang
      <span class="error">orthopedic</span>
      surgeon okay Dr. Jackson Avery practically
    </span>
    Alex approximately Isobel Meredith's father Jackson obviously a surgical resident really
  </p>
</template>

<script setup>
  import {Cloudy, Close, BrushFilled } from '@element-plus/icons-vue'
  import { ref } from 'vue'

  const isEdit = ref(false);

  const handleSelect = () => {
    const selectText = window.getSelection().toString();

    isEdit.value = selectText ? true : false;
  };

  const addCircle = () => {
    const selectRange = window.getSelection().getRangeAt(0);
    const circleTag = document.createElement("span");
    circleTag.className = 'error';

    selectRange.surroundContents(circleTag);
  };

  const addDelete = () => {
    const selectRange = window.getSelection().getRangeAt(0);
    const delTag = document.createElement("del");

    selectRange.surroundContents(delTag);
  };

  const addHighlight = () => {
    const selectRange = window.getSelection().getRangeAt(0);
    const highlightTag = document.createElement("span");
    highlightTag.className = 'highlight';

    selectRange.surroundContents(highlightTag);
  };
</script>

<style>
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
    background-color: #d7a7cd;
    background-image: linear-gradient(to right, #b285bf 0%, #d7a7cd 100%);
  }

  .error {
    border: 3px solid #b71919;
    border-radius: 50%;
    border-width: medium thin thick 10px;
  }

  del {
    text-decoration: line-through;
    background-color: #b719192b;
    color: #a12323;
  }

  ::-moz-selection,
  ::selection {
    background-color: #d7a7cd;
    background-image: linear-gradient(to right, #b285bf 0%, #d7a7cd 100%);
  }
</style>

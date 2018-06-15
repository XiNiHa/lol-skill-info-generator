<template>
  <div id="app">
    <app-skill-generator @change="processOutput"/>
    <textarea id="output"></textarea>
  </div>
</template>

<script>
import appSkillGenerator from './components/SkillGenerator.vue'

export default {
  name: 'App',
  components: {
    appSkillGenerator
  },
  methods: {
    processOutput () {
      setTimeout(() => {
        let wrapperObject = document.getElementsByClassName('skills-wrapper')[0]
        document.getElementById('output').value = wrapperObject.innerHTML.replace(/ data-v-[^\s]+=""/g, '')
      }, 10)
    }
  }
}
</script>

<style>
/* only for app */
#app {
  display: flex;
}

#skill-generator {
  width: 700px
}

#output {
  flex-grow: 1;
}

/* common */
.skills-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.skills-master-order {
  display: flex;
  align-items: center;
  margin: 5px;
}

.skills-master-order span {
  margin: 10px;
}

.skill-icon {
  width: 60px;
  height: 60px;
}

.skill-wrapper {
  border: 1px solid #c9c9c9
}

.skill-letter {
  text-align: center;
}

.skills-select-table {
  margin: 5px;
  border: 1px solid #c9c9c9;
  text-align: center;
  border-collapse: collapse
}

.skills-table-header {
  background: #fff;
  border: 1px solid #999;
  width: 25px;
  height: 25px;
}

.skills-table-cell {
  background: #f9f9f9;
  border: 1px solid #d9d9d9;
  width: 25px;
  height: 25px;
}

.skills-table-cell-active {
  background: rgb(59, 183, 255);
}
</style>

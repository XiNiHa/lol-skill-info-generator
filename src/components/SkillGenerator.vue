<template>
  <div id="skill-generator">
    <select v-model="champion">
      <option v-for="(champ, key, i) in champions" :key="i" :value="key">{{ champ.name }}</option>
    </select>
    <div v-if="currentChampion !== undefined" class="skills-wrapper">
      <div v-if="!skillsMasterOrder.includes(-1)" class="skills-master-order">
        <div class="skill-wrapper">
          <img :src="'https://ddragon.leagueoflegends.com/cdn/8.13.1/img/spell/' + currentChampion.spells[skillsMasterOrder[0]].image.full" class="skill-icon">
          <div class="skill-letter">{{ skillNumToLetter(skillsMasterOrder[0]) }}</div>
        </div>
        <span>></span>
        <div class="skill-wrapper">
          <img :src="'https://ddragon.leagueoflegends.com/cdn/8.13.1/img/spell/' + currentChampion.spells[skillsMasterOrder[1]].image.full" class="skill-icon">
          <div class="skill-letter">{{ skillNumToLetter(skillsMasterOrder[1]) }}</div>
        </div>
        <span>></span>
        <div class="skill-wrapper">
          <img :src="'https://ddragon.leagueoflegends.com/cdn/8.13.1/img/spell/' + currentChampion.spells[skillsMasterOrder[2]].image.full" class="skill-icon">
          <div class="skill-letter">{{ skillNumToLetter(skillsMasterOrder[2]) }}</div>
        </div>
        <span>></span>
        <div class="skill-wrapper">
          <img :src="'https://ddragon.leagueoflegends.com/cdn/8.13.1/img/spell/' + currentChampion.spells[skillsMasterOrder[3]].image.full" class="skill-icon">
          <div class="skill-letter">{{ skillNumToLetter(skillsMasterOrder[3]) }}</div>
        </div>
      </div>
      <div>
        <table class="skills-select-table">
          <thead>
            <th v-for="i in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]" :key="i" class="skills-table-header">{{ i }}</th>
          </thead>
          <tbody>
            <tr v-for="i in [0, 1, 2, 3]" :key="i">
              <td v-for="j in [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17]" :key="j" :class="{'skills-table-cell': true, 'skills-table-cell-active': skillsSelectOrder[j] === i}" @click="$emit('change'); Vue.set(skillsSelectOrder, j, i); evalMasterOrder()">{{ skillNumToLetter(i) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import Axios from 'axios'

export default {
  data () {
    return {
      Vue,
      skillsMasterOrder: [-1, -1, -1, -1],
      skillsSelectOrder: [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1],
      champion: '',
      currentChampion: undefined,
      champions: []
    }
  },
  watch: {
    champion () {
      Axios.get('https://ddragon.leagueoflegends.com/cdn/8.13.1/data/ko_KR/champion/<name>.json'.replace(/<name>/, this.champion)).then(res => {
        this.currentChampion = res.data.data[this.champion]
      })
    }
  },
  mounted () {
    Axios.get('https://ddragon.leagueoflegends.com/cdn/8.13.1/data/ko_KR/champion.json').then(res => {
      this.champions = res.data.data
    })
  },
  methods: {
    skillNumToLetter (num) {
      switch (num) {
        case 0:
          return 'Q'
        case 1:
          return 'W'
        case 2:
          return 'E'
        case 3:
          return 'R'
      }
    },
    evalMasterOrder () {
      if (this.skillsSelectOrder.includes(-1)) {
        return
      }
      let currentOrder = 0
      let counter = [0, 0, 0, 0]
      for (let i of this.skillsSelectOrder) {
        counter[i]++
        if (counter[i] === this.currentChampion.spells[i].maxrank) {
          this.skillsMasterOrder[currentOrder] = i
          currentOrder++
        }
      }
      if (this.skillsSelectOrder[17] !== this.skillsMasterOrder[3]) {
        this.skillsMasterOrder[3] = this.skillsSelectOrder[17]
      }
    }
  }
}
</script>

<style scoped>
</style>

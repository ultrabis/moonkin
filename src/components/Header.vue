<template>
  <section class="section has-background-black-ter is-paddingless">
    <nav class="level">
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">DPS</p>
          <p class="title">{{ dps }}</p>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">Spell Hit Weight</p>
          <p class="title">{{ spellHitWeight }}</p>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">Spell Crit Weight</p>
          <p class="title">{{ spellCritWeight }}</p>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">Int Weight</p>
          <p class="title">{{ intWeight }}</p>
        </div>
      </div>
    </nav>
  </section>
</template>

<style scoped>
.level-item {
  color: white;
}
.title {
  color: white;
}
.no-borders {
  border-left: none;
  border-right: none;
  border-radius: 0px;
}
</style>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import libclassic from 'libclassic'

const Props = Vue.extend({
  props: {
    data: libclassic.Cast
  }
})

@Component
export default class Header extends Props {
  get dps() {
    let val = this.data.dps.effective.avg
    if (this.data.spell.isInsectSwarm || this.data.spell.isHurricane) {
      val = this.data.periodicDPS.effective.dps
    }
    return `${libclassic.utils.roundedString(val, 2)}`
  }

  get spellHitWeight() {
    if (this.data.spell.isInsectSwarm || this.data.spell.isHurricane) {
      return `N/A`
    }
    return `${libclassic.utils.roundedString(this.data.spellHitWeight, 2)}`
  }

  get spellCritWeight() {
    if (this.data.spell.isInsectSwarm || this.data.spell.isHurricane) {
      return `N/A`
    }
    return `${libclassic.utils.roundedString(this.data.spellCritWeight, 2)}`
  }

  get intWeight() {
    if (this.data.spell.isInsectSwarm || this.data.spell.isHurricane) {
      return `N/A`
    }

    return `${libclassic.utils.roundedString(this.data.intWeight, 3)} (${libclassic.utils.roundedString(1 / this.data.intWeight, 2)})`
  }
}
</script>

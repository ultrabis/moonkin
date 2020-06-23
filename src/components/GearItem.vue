<template>
  <div>
    <template v-if="isLeftColumn">
      <div class="gear-item">
        <div class="gear-item-image">
          <div
            class="image-wrapper"
            @click="mouseClick"
            @mouseenter="mouseEnter(itemSlot)"
            @mousemove="mouseMove"
            @mouseleave="mouseLeave"
          >
            <figure class="image is-48x48">
              <img :src="iconPath" style="zIndex: 3" />
            </figure>
            <font-awesome-icon
              v-if="isLocked"
              class="is-overlay"
              :icon="['fas', 'lock']"
              :style="{ padding: '2px', color: 'white' }"
            />
          </div>
        </div>
        <div class="gear-item-text">
          <p
            :class="itemData.qualityName.toLowerCase()"
            class="gear-item-text-name left-text has-text-left is-size-7-mobile"
          >
            {{ itemData.name }}
          </p>
          <p
            v-if="!itemData.isEmpty"
            :class="itemData.enchantClass"
            class="gear-item-text-enchant left-text has-text-left is-size-7"
            @click="enchantClick"
          >
            {{ itemData.enchantText }}
          </p>
        </div>
      </div>
    </template>
    <template v-else>
      <div class="gear-item gear-item-right">
        <div class="gear-item-text">
          <p
            :class="itemData.qualityName.toLowerCase()"
            class="gear-item-text-name right-text has-text-right is-size-7-mobile"
          >
            {{ itemData.name }}
          </p>
          <p
            v-if="!itemData.isEmpty"
            :class="itemData.enchantClass"
            class="gear-item-text-enchant right-text has-text-right is-size-7"
            @click="enchantClick"
          >
            {{ itemData.enchantText }}
          </p>
        </div>
        <div class="gear-item-image">
          <div
            class="image-wrapper"
            @click="mouseClick"
            @mouseenter="mouseEnter(itemSlot)"
            @mousemove="mouseMove"
            @mouseleave="mouseLeave"
          >
            <figure class="image is-48x48">
              <img :src="iconPath" style="zIndex: 1" />
            </figure>
            <font-awesome-icon
              v-if="isLocked"
              class="is-overlay"
              :icon="['fas', 'lock']"
              :style="{ padding: '2px', color: 'white' }"
            />
          </div>
        </div>
      </div>
    </template>
    <GearItemTooltip :class="{ hidden: !showToolTip }" :style="toolTipStyle" :itemData="itemData" />
  </div>
</template>

<style scoped>
.gear-item-right {
  text-align: right;
  justify-content: flex-end;
}
.gear-item {
  display: flex;
  width: 100%;
  overflow: hidden;
  overflow-x: hidden;
  overflow-y: hidden;
}

.gear-item-image {
  display: block;
  position: relative;
  flex-shrink: 0;
  padding: 2px;
}

.gear-item-text {
  overflow: hidden;
}

.right-text {
  margin-right: 0.625rem;
}

.left-text {
  margin-left: 0.625rem;
}

.gear-item-text-name {
  margin-bottom: 0.25rem;

  line-height: 1.2;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.gear-item-text-enchant {
  margin-bottom: 0.25rem;
  line-height: 1.2;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.image-wrapper {
  border: 0.0625rem solid #595959;
}
</style>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import libclassic from 'libclassic'
import GearItemTooltip from './GearItemTooltip.vue'

const Props = Vue.extend({
  props: {
    options: Object,
    itemSlot: Number,
    itemData: Object
  }
})

@Component({
  components: { GearItemTooltip }
})
export default class GearItem extends Props {
  showToolTip = false
  toolTipStyle = {
    zIndex: 999,
    position: 'fixed',
    top: '0px',
    left: '0px'
  }

  get iconPath() {
    return `${process.env.BASE_URL}icons/${this.itemData.icon}`
  }

  get isLocked() {
    switch (this.itemSlot) {
      case libclassic.ItemSlot.Head:
        return this.options.equipment.lockedItems.head ? true : false
      case libclassic.ItemSlot.Hands:
        return this.options.equipment.lockedItems.hands ? true : false
      case libclassic.ItemSlot.Neck:
        return this.options.equipment.lockedItems.neck ? true : false
      case libclassic.ItemSlot.Waist:
        return this.options.equipment.lockedItems.waist ? true : false
      case libclassic.ItemSlot.Shoulder:
        return this.options.equipment.lockedItems.shoulder ? true : false
      case libclassic.ItemSlot.Legs:
        return this.options.equipment.lockedItems.legs ? true : false
      case libclassic.ItemSlot.Back:
        return this.options.equipment.lockedItems.back ? true : false
      case libclassic.ItemSlot.Feet:
        return this.options.equipment.lockedItems.feet ? true : false
      case libclassic.ItemSlot.Chest:
        return this.options.equipment.lockedItems.chest ? true : false
      case libclassic.ItemSlot.Wrist:
        return this.options.equipment.lockedItems.wrist ? true : false
      case libclassic.ItemSlot.Finger:
        return this.options.equipment.lockedItems.finger ? true : false
      case libclassic.ItemSlot.Finger2:
        return this.options.equipment.lockedItems.finger2 ? true : false
      case libclassic.ItemSlot.Mainhand:
        return this.options.equipment.lockedItems.mainhand ? true : false
      case libclassic.ItemSlot.Offhand:
        return this.options.equipment.lockedItems.offhand ? true : false
      case libclassic.ItemSlot.Trinket:
        return this.options.equipment.lockedItems.trinket ? true : false
      case libclassic.ItemSlot.Trinket2:
        return this.options.equipment.lockedItems.trinket2 ? true : false
      case libclassic.ItemSlot.Relic:
        return this.options.equipment.lockedItems.idol ? true : false
      default:
        return false
    }
  }

  get isLeftColumn() {
    switch (this.itemSlot) {
      case libclassic.ItemSlot.Head:
      case libclassic.ItemSlot.Neck:
      case libclassic.ItemSlot.Shoulder:
      case libclassic.ItemSlot.Back:
      case libclassic.ItemSlot.Chest:
      case libclassic.ItemSlot.Wrist:
      case libclassic.ItemSlot.Mainhand:
      case libclassic.ItemSlot.Offhand:
      case libclassic.ItemSlot.Ranged:
        return true
      default:
        return false
    }
  }

  mouseClick(): void {
    this.options.equipment.itemSearchSlot = this.itemSlot
  }

  enchantClick(): void {
    this.options.equipment.enchantSearchSlot = this.itemSlot
  }

  mouseEnter(slot: number): void {
    if (!this.itemData.isEmpty && libclassic.utils.isMobile()) {
      this.showToolTip = true
    }
  }

  mouseMove(event: any): void {
    if (this.isLeftColumn) {
      this.toolTipStyle.left = event.clientX + 40 + 'px'
      this.toolTipStyle.top = event.clientY - 30 + 'px'
    } else {
      this.toolTipStyle.left = event.clientX - 300 + 'px'
      this.toolTipStyle.top = event.clientY - 30 + 'px'
    }
  }

  mouseLeave(event: any): void {
    this.showToolTip = false
  }
}
</script>

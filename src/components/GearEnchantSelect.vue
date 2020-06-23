<template>
  <div>
    <div class="enchantSelect">
      <b-modal :active.sync="showEnchants" scroll="keep">
        <div class="container is-fluid">
          <b-table
            @select="selectEnchant"
            :data="enchantData"
            :columns="enchantColumns"
            :paginated="paginated"
            :pagination-simple="true"
            per-page="5"
            sort-icon="arrow-up"
            sort-icon-size="is-small"
            aria-next-label="Next page"
            aria-previous-label="Previous page"
            aria-page-label="Page"
            aria-current-label="Current page"
            default-sort="['score', 'desc']"
            hoverable
          >
            <template slot="footer">
              <GearButtonBar
                :options="buttonBarOptions()"
                @unlock-gear="unlockEnchant"
                @unequip-gear="unequipEnchant"
              />
            </template>
          </b-table>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<style scoped></style>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import libclassic from 'libclassic'
import GearButtonBar from './GearButtonBar.vue'

const GearEnchantSelectProps = Vue.extend({
  props: {
    options: Object,
    encounter: Object
  }
})

@Component({
  components: { GearButtonBar }
})
export default class GearEnchantSelect extends GearEnchantSelectProps {
  paginated = true

  buttonBarOptions() {
    return {
      showShare: false,
      showUnlock: this.enchantLocked(),
      showUnequip: this.enchantEquipped()
    }
  }

  enchantEquipped() {
    return libclassic.locked.enchantEquipped(this.options.equipment.lockedEnchants, this.options.equipment.enchantSearchSlot)
  }

  enchantLocked() {
    return libclassic.locked.enchantLocked(this.options.equipment.lockedEnchants, this.options.equipment.enchantSearchSlot)
  }

  unequipEnchant() {
    libclassic.locked.unequipEnchant(this.options.equipment.lockedEnchants, this.options.equipment.enchantSearchSlot)
    this.options.equipment.enchantSearchSlot = libclassic.ItemSlot.None
  }

  unlockEnchant() {
    libclassic.locked.unlockEnchant(this.options.equipment.lockedEnchants, this.options.equipment.enchantSearchSlot)
    this.options.equipment.enchantSearchSlot = libclassic.ItemSlot.None
  }

  selectEnchant(obj1: any, obj2: any) {
    libclassic.locked.setEnchant(this.options.equipment.lockedEnchants, this.options.equipment.enchantSearchSlot, obj1.customId)
    this.options.equipment.enchantSearchSlot = libclassic.ItemSlot.None
  }

  get showEnchants() {
    if (this.options.equipment.enchantSearchSlot !== libclassic.ItemSlot.None) {
      return true
    }
    return false
  }

  set showEnchants(slot) {
    this.options.equipment.enchantSearchSlot = libclassic.ItemSlot.None
  }

  get enchantData() {
    return this.encounter.enchants
  }

  get enchantColumns() {
    return [
      {
        field: 'name',
        label: 'name',
        width: '300',
        numeric: false,
        sortable: true,
        searchable: true
      },
      {
        field: 'spellDamage',
        label: 'Spell Dmg',
        width: '20',
        numeric: true,
        sortable: true,
        searchable: false
      },
      {
        field: 'arcaneDamage',
        label: 'Arcane Dmg',
        width: '20',
        numeric: true,
        sortable: true,
        searchable: false
      },
      {
        field: 'natureDamage',
        label: 'Nature Dmg',
        width: '20',
        numeric: true,
        sortable: true,
        searchable: false
      },
      {
        field: 'spellCrit',
        label: 'Spell Crit',
        width: '20',
        numeric: true,
        sortable: true,
        searchable: false
      },
      {
        field: 'spellHit',
        label: 'Spell Hit',
        width: '20',
        numeric: true,
        sortable: true,
        searchable: false
      },
      {
        field: 'score',
        label: 'score',
        width: '20',
        numeric: true,
        sortable: true,
        searchable: false
      }
    ]
  }
}
</script>

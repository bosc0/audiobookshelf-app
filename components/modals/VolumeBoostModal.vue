<template>
  <modals-modal v-model="show" :width="200" height="100%">
    <template #outer>
      <div class="absolute top-8 left-4 z-40">
        <p class="text-white text-2xl truncate">{{ $strings.LabelVolumeBoost }}</p>
      </div>
    </template>

    <div class="w-full h-full overflow-hidden absolute top-0 left-0 flex items-center justify-center">
      <div class="w-full overflow-x-hidden overflow-y-auto bg-primary rounded-lg border border-border" style="max-height: 75%" @click.stop>
        <p v-if="unavailable" class="p-3 text-fg-muted text-sm" role="status">{{ $strings.MessageVolumeBoostUnavailable }}</p>
        <ul class="w-full" role="listbox" aria-labelledby="listbox-label">
          <template v-for="gain in gains">
            <li :key="gain" class="text-fg select-none relative py-4" :class="gain === selected ? 'bg-bg-hover/50' : ''" role="option" @click="clickedOption(gain)">
              <div class="flex items-center justify-center">
                <span class="font-normal block truncate text-lg">{{ gainLabel(gain) }}</span>
              </div>
            </li>
          </template>
        </ul>
        <div class="flex items-center justify-center py-3 border-t border-fg/10">
          <button :disabled="!canDecrement" @click="decrement" class="icon-num-btn w-8 h-8 text-fg-muted rounded border border-border flex items-center justify-center">
            <span class="material-symbols">remove</span>
          </button>
          <div class="w-24 text-center">
            <p class="text-xl">{{ gainLabel(selected) }}</p>
          </div>
          <button :disabled="!canIncrement" @click="increment" class="icon-num-btn w-8 h-8 text-fg-muted rounded border border-border flex items-center justify-center">
            <span class="material-symbols">add</span>
          </button>
        </div>
      </div>
    </div>
  </modals-modal>
</template>

<script>
export default {
  props: {
    value: Boolean,
    unavailable: Boolean,
    volumeBoost: Number
  },
  data() {
    return {
      MIN_BOOST: 0,
      MAX_BOOST: 15
    }
  },
  computed: {
    show: {
      get() {
        return this.value
      },
      set(val) {
        this.$emit('input', val)
      }
    },
    selected: {
      get() {
        return this.volumeBoost
      },
      set(val) {
        this.$emit('update:volumeBoost', val)
      }
    },
    gains() {
      return [0, 2, 4, 6, 9, 12]
    },
    canIncrement() {
      return this.selected + 1 <= this.MAX_BOOST
    },
    canDecrement() {
      return this.selected - 1 >= this.MIN_BOOST
    }
  },
  methods: {
    gainLabel(gain) {
      return gain === 0 ? this.$strings.LabelOff : `+${gain} dB`
    },
    increment() {
      if (this.canIncrement) this.selected = this.selected + 1
    },
    decrement() {
      if (this.canDecrement) this.selected = this.selected - 1
    },
    clickedOption(gain) {
      this.selected = gain
      this.show = false
    }
  }
}
</script>

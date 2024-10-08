<template>
  <div class="race">
    <div class="boxes">
      <div class="positions-container">
        <RacerList :racers="globalStore.activeRace.positions" />
      </div>
      <div class="blocks-container">
        <div class="blocks">
          <div class="box" v-if="globalStore.activeRace.totalRacers && globalStore.activeRace.totalRacers !== 1">
            <span class="hud-text split big"
              ><span class="leftAligned">{{ translate('pos') }}: </span
              ><span id="race-position">{{
                `${globalStore.activeRace.position}/${globalStore.activeRace.totalRacers}`
              }}</span></span
            >
          </div>
          <div class="box">
            <span class="hud-text split"
              ><span class="leftAligned">{{ translate('checkpoints') }}:</span
              ><span id="race-checkpoints">
                {{
                  `${globalStore.activeRace.currentCheckpoint}/${globalStore.activeRace.totalCheckpoints}`
                }}</span
              ></span
            >
          </div>
          <div class="box">
            <span class="hud-text split"
              ><span class="leftAligned">{{ translate('lap') }}:</span
              ><span id="race-lap">
                {{lapText }}</span
              ></span
            >
          </div>
          <div class="box">
            <span class="hud-text split"
              ><span class="leftAligned">{{ translate('current') }}:</span>
              <span id="race-time">{{ secondsToHMS(globalStore.activeRace.time) }}</span></span
            >
          </div>
          <div class="box">
          <span class="hud-text split"
            ><span class="leftAligned">{{ translate('best') }}:</span>
            <span id="race-besttime"> {{ secondsToHMS(globalStore.activeRace.bestLap) }}</span></span
          >
        </div>
        <div class="box">
          <span class="hud-text split"
            ><span class="leftAligned">{{ translate('total') }}:</span>
            <span id="race-totaltime">{{ secondsToHMS(globalStore.activeRace.totalTime) }}</span></span
          >
        </div>
    <div class="box">
          <span class="hud-text split"
            ><span class="leftAligned">{{ translate('track') }} :</span>
            <span id="race-racename">{{ globalStore.activeRace.raceName }}</span></span
          >
        </div>
        <div id="race-ghosted-span">
          <span id="race-ghosted-value">{{
            globalStore.activeRace.ghosted ? "👻" : ""
          }}</span>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import RacerList from "./RacerList.vue";
import { useGlobalStore } from "@/store/global";
import { secondsToHMS } from "@/helpers/secondsToHMS";
import { computed } from "vue";
import { translate } from "@/helpers/translate";
const globalStore = useGlobalStore();

const hudpositionToCss: Record<string, string> = {
  'split': 'space-between',
  'left': 'start',
  'right': 'end',
}

const placement = computed(() => globalStore.baseData?.data?.hudSettings?.location ? hudpositionToCss[globalStore.baseData?.data?.hudSettings?.location]: 'space-between')
const direction = computed(() => globalStore.baseData?.data?.hudSettings?.location === 'left' ? 'row-reverse': 'row')
const lapText = computed(() => {
  if (globalStore.activeRace.totalLaps === 0) return 'Sprint'
  else if (globalStore.activeRace.totalLaps === -1) return  `${globalStore.activeRace.currentLap}/-`
  return `${globalStore.activeRace.currentLap}/${globalStore.activeRace.totalLaps}`
})
</script>

<style scoped lang="scss">
#race-ghosted-span {
  display: flex;
  justify-content: end;
}
.race {
    position: absolute;
    top: 5vh;
    left: 0;
    z-index: 100;
}
.boxes {
  display: flex;
  justify-content: v-bind(placement);
  flex-direction: v-bind(direction);
  width: 100vw;
}
.blocks-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  gap: 20px;
  margin: 10px;
}

.positions-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin: 10px;
}

.blocks {
  width: 25em;
  flex-grow: 4;
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.box {
  background: $hud-background;
  width: 100%;
  display: flex;
  gap: 1em;
  align-items: center;
  padding-right: 1rem;
}

.hud-text {
    text-align: right;
    padding: 10px;
    padding-right: 15px;
    padding-left: 15px;
    font-weight: 600;
    width: 100%;
    text-transform: uppercase;
}

.leftAligned {
    text-align: left;
}

.split {
    display: flex;
    justify-content: space-between;
}


</style>

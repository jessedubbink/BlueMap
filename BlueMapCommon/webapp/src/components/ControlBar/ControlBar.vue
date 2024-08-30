<template>
  <div class="control-bar">
    <MenuButton :close="appState.menu.isOpen" :back="false" @action="appState.menu.reOpenPage()" :title="$t('menu.tooltip')" />
    <div class="space thin-hide"></div>
    <SvgButton v-if="appState.maps.length > 1" class="thin-hide" :title="$t('maps.tooltip')"
               @action="appState.menu.openPage('maps', $t('maps.title'))">
      <svg viewBox="0 0 30 30">
        <polygon points="26.708,22.841 19.049,25.186 11.311,20.718 3.292,22.841 7.725,5.96 13.475,4.814 19.314,7.409 25.018,6.037 "/>
      </svg>
    </SvgButton>
    <div class="space thin-hide greedy"></div>
    <DayNightSwitch v-if="showMapMenu" class="thin-hide" :title="$t('lighting.dayNightSwitch.tooltip')" />
    <div class="space thin-hide"></div>
    <ControlsSwitch v-if="showMapMenu" class="thin-hide"></ControlsSwitch>
    <div class="space thin-hide"></div>
    <SvgButton v-if="showMapMenu" class="thin-hide" :title="$t('resetCamera.tooltip')" @action="$bluemap.resetCamera()">
      <svg viewBox="0 0 30 30">
        <rect x="7.085" y="4.341" transform="matrix(0.9774 0.2116 -0.2116 0.9774 3.2046 -1.394)" width="2.063" height="19.875"/>
        <path d="M12.528,5.088c0,0,3.416-0.382,4.479-0.031c1.005,0.332,2.375,2.219,3.382,2.545c1.096,0.354,4.607-0.089,4.607-0.089
      l-2.738,8.488c0,0-3.285,0.641-4.344,0.381c-1.049-0.257-2.607-2.015-3.642-2.324c-0.881-0.264-3.678-0.052-3.678-0.052
      L12.528,5.088z"/>
      </svg>
    </SvgButton>
    <PositionInput v-if="showMapMenu" class="pos-input" />
    <Compass v-if="showMapMenu" :title="$t('compass.tooltip')" />
  </div>
</template>

<script>
  import PositionInput from "./PositionInput.vue";
  import Compass from "./Compass.vue";
  import DayNightSwitch from "./DayNightSwitch.vue";
  import ControlsSwitch from "./ControlsSwitch.vue";
  import MenuButton from "./MenuButton.vue";
  import SvgButton from "./SvgButton.vue";

  export default {
    name: "ControlBar",
    components: {
      SvgButton,
      MenuButton,
      ControlsSwitch,
      DayNightSwitch,
      PositionInput,
      Compass
    },
    data() {
      return {
        appState: this.$bluemap.appState,
        mapViewer: this.$bluemap.mapViewer.data
      }
    },
    computed: {
      showMapMenu() {
        return this.mapViewer.mapState === "loading" || this.mapViewer.mapState === "loaded";
      },
    },
  }
</script>

<style lang="scss">
@import "/src/scss/variables.scss";

  .control-bar {
    position: fixed;
    top: 0;
    left: 0;

    display: flex;

    filter: drop-shadow(1px 1px 3px rgba(0, 0, 0, 0.53));
    height: 2em;

    margin: 0.5em;
    width: calc(100% - 1em);

    .pos-input {
      max-width: 20em;
      width: 100%;
    }

    > :not(:first-child) {
      border-left: solid 1px var(--theme-bg-light);
    }

    .space {
      width: 0.5em;
      flex-shrink: 0;

      &.greedy {
        flex-grow: 1;
      }
    }

    .space, .space + * {
      border-left: none;
    }

    @media (max-width: $mobile-break) {
      margin: 0;
      width: 100%;

      background-color: var(--theme-bg);

      .pos-input {
        max-width: unset;
      }

      .thin-hide {
        display: none;
      }

      .space {
        width: 1px;
      }
    }

  }
</style>
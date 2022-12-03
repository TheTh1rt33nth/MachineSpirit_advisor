<template>
  <div class="container">
    <div class="log-generator">
      <div class="row">
        <!-- Player names -->
        <div class="col-md-2 offset-md-1">
          <select class="form-select" v-model="player1.playerName">
            <option value="" selected disabled hidden>Select a player</option>
            <option v-for="player in importedData.players" :value="player.value">
              {{ player.text }}
            </option>
          </select>
          <select class="form-select" v-model="player2.playerName">
            <option value="" selected disabled hidden>Select a player</option>
            <option v-for="player in importedData.players" :value="player.value">
              {{ player.text }}
            </option>
          </select>
        </div>
        <!-- Mission description TODO: EMIT FROM GAME SIZE CHANGE-->
        <div class="col-md-2">
          <select class="form-select" v-model="gameSize">
            <option value="" selected disabled hidden>Select a gamesize</option>
            <option v-for="gsize in importedData.gameSizes" :value="gsize.value">
              {{ gsize.text }}
            </option>
          </select>

          <select class="form-select" :disabled=!isSizeSet v-model="missionName">
            <option value="" selected disabled hidden>Select a mission</option>
            <option v-for="gtype in importedData.missions[gameSize]" :value="gtype.value">
              {{ gtype.text }}
            </option>
          </select>
          <input type="text" v-if="missionName == 'Custom'" v-model="customName" placeholder="Custom mission name"
            class="form-control" />


        </div>

        <!-- Game type -->
        <div class="col-md-2">
          <select class="form-select" v-model="player1.gamePlayed">
            <option value="" selected disabled hidden>Select a gametype</option>
            <option v-for="gtype in importedData.gameTypes" :value="gtype.value">
              {{ gtype.text }}
            </option>
          </select>
          <select class="form-select" v-model="player2.gamePlayed">
            <option value="" selected disabled hidden>Select a gametype</option>
            <option v-for="gtype in importedData.gameTypes" :value="gtype.value">
              {{ gtype.text }}
            </option>
          </select>
        </div>
        <!-- Victory points -->
        <div class="col-md-2">
          <input type="number" min="0" max="100" v-model="player1.victoryPoints" class="form-control" />
          <input type="number" min="0" max="100" v-model="player2.victoryPoints" class="form-control" />
        </div>

        <!-- Victor -->
        <div class="col-md-2">

        </div>

      </div>
    </div>
  </div>

</template>


<script>
import logGeneratorJson from "./logGeneratorData.json"
export default {
  data() {
    return {
      player1: {
        playerName: "",
        gamePlayed: "",
        victoryPoints: 0
      },
      player2: {
        playerName: "",
        gamePlayed: "",
        victoryPoints: 0
      },
      gameSize: "",
      missionName: "",
      customName: "",
      importedData: logGeneratorJson
    }
  },
  computed: {
    isSizeSet() {
      console.log((this.gameSize && this.gameSize.length !== 0));
      return (this.gameSize && this.gameSize.length !== 0);
    }
  }
}
</script>
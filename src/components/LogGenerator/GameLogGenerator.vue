<template>
  <div class="container-fluid my-3">
    <div class="log-generator">
      <div class="row justify-content-center">
        <!-- Header -->
        <div class="col-md-5">
          <h2>Generate report for a game</h2>
        </div>
        <div class="col-md-2">
          <input type="date" class="form-control" v-model="gameDate" />
        </div>
      </div>
      <div class="row align-items-center">
        <!-- Player names -->
        <div class="col-md-2 offset-md-1">
          <select class="form-select" v-model="player1.playerName">
            <option value="Player 1" selected disabled hidden>Select a player</option>
            <option v-for="player in importedData.players" :value="player.value">
              {{ player.text }}
            </option>
          </select>
          <select class="form-select" v-model="player2.playerName">
            <option value="Player 2" selected disabled hidden>Select a player</option>
            <option v-for="player in importedData.players" :value="player.value">
              {{ player.text }}
            </option>
          </select>
        </div>
        <!-- Mission type -->
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
        <div class="col-md-1">
          <input type="number" min="0" max="100" v-model="player1.victoryPoints" class="form-control" />
          <input type="number" min="0" max="100" v-model="player2.victoryPoints" class="form-control" />
        </div>

        <!-- Victor -->
        <div class="col-md-2">
          <h3>{{ victorString }}</h3>
        </div>

      </div>
      <div class="row justify-content-center">
        <div class="col-md-7">
          <input type="text" v-model="generatedLogMessage" readonly placeholder="Fill more fields to get a log message" class="form-control" />
        </div>
        <div class="col-md-1">
          <button @click="copyToClipboard()" class="btn btn-light">Copy</button>
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
        playerName: "Player 1",
        gamePlayed: "",
        victoryPoints: 0
      },
      player2: {
        playerName: "Player 2",
        gamePlayed: "",
        victoryPoints: 0
      },
      gameDate: "",
      gameSize: "",
      missionName: "",
      customName: "",
      importedData: logGeneratorJson
    }
  },
  computed: {
    isSizeSet() {
      return (this.gameSize && this.gameSize.length !== 0);
    },
    victorString() {
      if (this.player1.victoryPoints > this.player2.victoryPoints) return "Victor: " + this.player1.playerName;
      else if (this.player1.victoryPoints < this.player2.victoryPoints) return "Victor: " + this.player2.playerName;
      else return "Draw";
    },
    generatedLogMessage() {
      if (this.isEmpty(this.missionName)) return "";
      let result = "";
      result += `[${this.setGameDate}] ### `;
      result += `${this.player1.playerName} vs ${this.player2.playerName} // `;
      result += `${this.missionName} // `;
      result += `${this.formatGametype()}`;
      result += `VP - ${this.player1.victoryPoints}:${this.player2.victoryPoints} // `;
      result += `${this.victorString} ### `;
      return result;
    },
    setGameDate() {
      if (this.gameDate) return this.formatDate(new Date(this.gameDate))
      else return this.formatDate(new Date(Date.now()));
    }
  },
  methods: {
    formatDate(dateToFormat) {
      return `${dateToFormat.getDate()}.${dateToFormat.getMonth() + 1}.${dateToFormat.getFullYear()}`;
    },
    formatGametype() {
      let result = "";
      if (!this.isEmpty(this.player1.gamePlayed)) {
        result += `${this.player1.gamePlayed}(${this.player1.playerName})`;
      }
      if (!this.isEmpty(this.player2.gamePlayed)) {
        if (!this.isEmpty(this.player1.gamePlayed)) {
          result += " - "
        }
        result += `${this.player2.gamePlayed}(${this.player2.playerName})`;
      }
      if (!this.isEmpty(result)) result += " // "
      return result;
    },
    isEmpty(str) {
      return (!str || str.length === 0);
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.generatedLogMessage);
    }
  },
  watch: {
    gameSize() {
      this.missionName = "";
    }
  }
}
</script>
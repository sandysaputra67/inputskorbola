<template>
  <div id="app" class="container mt-5">
    <form @submit.prevent="saveMatch" class="needs-validation" novalidate>
      <div v-for="(match, index) in matches" :key="index" class="form-row mb-3">
        <div class="col">
          <input
            type="text"
            v-model="match.club1"
            class="form-control"
            placeholder="Club 1"
            required
          />
        </div>
        <div class="col">
          <input
            type="number"
            v-model="match.score1"
            min="0"
            class="form-control"
            placeholder="Score 1"
            required
          />
        </div>
        <div class="col">
          <input
            type="number"
            v-model="match.score2"
            min="0"
            class="form-control"
            placeholder="Score 2"
            required
          />
        </div>
      </div>
      <button @click="addMatch" class="btn btn-primary">Add Match</button>
      <button type="submit" class="btn btn-success">Save</button>
    </form>
    <div v-if="showResult" class="mt-4">
      <p>Team Points:</p>
      <ul class="list-group">
        <li
          v-for="(points, team) in teamPoints"
          :key="team"
          class="list-group-item"
        >
          {{ team }}: {{ points }} points
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function () {
    return {
      matches: [{ club1: "", score1: null, score2: null }],
      teamPoints: {},
      showResult: false,
    };
  },
  methods: {
    addMatch() {
      this.matches.push({ club1: "", score1: null, score2: null });
    },
    saveMatch() {
      // Validate the form before proceeding
      if (!this.validateForm()) {
        alert("Please fill all the fields for each match.");
        return;
      }

      this.calculatePoints();
      this.showResult = true;
    },
    validateForm() {
      return this.matches.every(
        (match) => match.club1 && match.score1 !== null && match.score2 !== null
      );
    },
    calculatePoints() {
      this.teamPoints = {};
      this.matches.forEach((match) => {
        const { club1, score1, score2 } = match;

        if (!this.teamPoints[club1]) {
          this.teamPoints[club1] = 0;
        }

        if (score1 > score2) {
          this.teamPoints[club1] += 3; // Club1 wins, gets 3 points
        } else if (score1 === score2) {
          this.teamPoints[club1] += 1; // It's a draw, gets 1 point
        } else {
          // Club1 loses, gets 0 points
        }
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

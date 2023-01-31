<template >
  <div class="p-4 md:p-10 md:container md:mx-auto">
    <div class="grid gap-3 md:grid-cols-3">
      <select v-model="selectedTeam" @change="filterByTeam(selectedTeam)"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 h-10 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
        <option value="">All Teams</option>
        <option v-for="team in teams" :key="team" :value="team">{{ team.name }}</option>
      </select>
      <button @click="sortByTotalPoints" type="button"
        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm h-10 px-5 py-2.5  mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Sort
        by Total Points</button>
      <button @click="randomizePlayers" type="button"
        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800 h-10">Reset
        Filters and Randomize</button>
    </div>

    <PlayerList :players="players" :filteredPlayers="filteredPlayers" :teams="teams" />
  </div>
</template>
<script>
import PlayerList from "@/components/PlayerList.vue";

export default {
  components: {
    PlayerList,
  },
  data() {
    return {
      players: [],
      filteredPlayers: [],
      selectedTeam: '',
      teams: []
    }
  },
  methods: {
    filterByTeam(team) {
      this.filteredPlayers = this.players.filter(player => player.team === team.id);
    },
    sortByTotalPoints() {
      this.filteredPlayers.sort((a, b) => b.totalPoints - a.totalPoints);
    },
    randomizePlayers() {
      this.filteredPlayers = this.players.sort(() => Math.random() - 0.5);
      this.selectedTeam = '';
    }
  },

  created() {
    fetch('http://localhost:3001')
      .then((response) => response.json())
      .then((data) => {
        this.players = data.elements.map((player) => {
          return {
            id: player.id,
            firstName: player.first_name,
            secondName: player.second_name,
            webName: player.web_name,
            totalPoints: player.total_points,
            team: player.team,
            formRank: player.form_rank,
            creativityRank: player.creativity_rank,
          };
        });
        this.teams = data.teams;
        this.filteredPlayers = this.players;
      });
  }
}
</script>
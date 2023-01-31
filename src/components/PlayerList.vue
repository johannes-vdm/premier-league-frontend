<template >
  <div class="grid grid-cols-1 gap-4 md:grid-cols-4 xl:grid-cols-4">
    <div v-for="player in filteredPlayers" :key="player.id" class="block w-full p-6 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700">
      <h1 class="text-xl">{{ player.webName }}</h1>
      <p>{{ player.firstName }} {{ player.secondName }}</p>
      <p>Team: {{ getTeamName(player.team) }}</p>
      <p>Total Points: {{ player.totalPoints }}</p>
      <p>Form Rank: {{ player.formRank }}</p>
      <p>Creativity Rank: {{ player.creativityRank }}</p>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    players: Array,
    filteredPlayers: Array,
    teams: Array
  },
  methods: {
    getTeamName(teamId) {
      const team = this.teams.find(team => team.id === teamId);
      return team ? team.name : '';
    },
  }
}
</script>

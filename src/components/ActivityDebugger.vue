<template>
  <div class="debugger">
    <h3 class="heading" @click="toggleShowData">Debugger</h3>
    <div class="debugger-inner" :class="{hidden: !showData}">
      <table>
        <caption>Current song</caption>
        <tr>
          <th>trackTitle</th>
          <th>userId</th>
          <th>trackId</th>
        </tr>
        <tr>
          <td>{{currentSong.trackTitle}}</td>
          <td>{{currentSong.userId}}</td>
          <td>{{currentSong.trackId}}</td>
        </tr>
      </table>
      <table>
        <caption>Users</caption>
        <tr>
          <th>name</th>
          <th>userId</th>
        </tr>
        <tr v-for="user in users" :key="'users' + user.userId">
          <td>{{user.name}}</td>
          <td>{{user.userId}}</td>
        </tr>
      </table>
      <!-- Add guesses -->
      <table>
        <caption>Player select submissions</caption>
        <tr>
          <th>userName</th>
          <th>selectedUser</th>
          <th>trackOwner</th>
        </tr>
        <tr v-for="s in selections" :key="s.userName">
          <td>{{s.userName}}</td>
          <td>{{s.selectedUser}}</td>
          <td>{{s.trackOwner}}</td>
        </tr>
      </table>
      <table>
        <caption>Scores</caption>
        <tr>
          <th>name</th>
          <th>userId</th>
          <th>score</th>
        </tr>
        <tr v-for="s in scores" :key="'scores' + s.userId">
          <td>{{s.name}}</td>
          <td>{{s.userId}}</td>
          <td>{{s.score}}</td>
        </tr>
      </table>
      <table>
        <caption>Songs</caption>
        <tr>
          <th>trackTitle</th>
          <th>userId</th>
          <th>trackId</th>
        </tr>
        <tr v-for="s in songs" :key="'songs' + s.userId">
          <td>{{s.trackTitle}}</td>
          <td>{{s.userId}}</td>
          <td>{{s.trackId}}</td>
        </tr>
      </table>
      <table>
        <caption>Played songs</caption>
        <tr>
          <th>trackTitle</th>
          <th>userId</th>
          <th>trackId</th>
        </tr>
        <tr v-for="s in playedSongs" :key="'played' + s.userId">
          <td>{{s.trackTitle}}</td>
          <td>{{s.userId}}</td>
          <td>{{s.trackId}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  name: "ActivityDebugger",
  props: ["users", "score", "currentSong", "songs", "playedSongs", "guesses"],
  data() {
    return {
      showData: false
    };
  },
  computed: {
    scores() {
      if (!this.score.length) return [];
      return this.users.map(user => {
        const { score } = this.score.find(
          ({ userId }) => userId === user.userId
        );
        return {
          name: user.name,
          userId: user.userId,
          score: score
        };
      });
    },
    selections() {
      if (!this.guesses) return [];

      const guesses = this.guesses.filter(guess => {
        const { trackId, userId } = this.currentSong;
        return guess.trackId === trackId && guess.trackOwnerId === userId;
      });

      return guesses.map(guess => ({
        trackId: guess.trackId,
        userName: this.users.find(({ userId }) => userId === guess.userId).name,
        trackOwner: this.users.find(
          ({ userId }) => userId === guess.trackOwnerId
        ).name,
        selectedUser: this.users.find(
          ({ userId }) => userId === guess.selectedUserId
        ).name
      }));
    }
  },
  methods: {
    toggleShowData() {
      this.showData = !this.showData;
    }
  }
};
</script>
<style lang="scss">
.debugger {
  position: fixed;
  top: 0;
  left: 0;
  overflow-y: scroll;
  background-color: #111;
  color: #fff;
  padding: 15px;
  border-bottom: 3px solid #111;
  h3 {
    font-size: 1rem;
    text-align: center;
    width: 100%;
    cursor: pointer;
  }
  * {
    color: inherit;
    margin-bottom: 0;
  }
  table {
    width: 100%;
    background-color: #f5f5f5;
    color: #111;
    margin-bottom: 10px;
    tr,
    td,
    th {
      padding: 5px;
    }
    tr {
      border-bottom: 1px solid #111;
    }
    td:nth-child(n + 2),
    th:nth-child(n + 2) {
      border-left: 1px solid #111;
    }
    caption {
      font-weight: 700;
      color: #fff;
      text-align: left;
    }
  }
}
.debugger-inner {
  height: 500px;
  width: 600px;
}
</style>

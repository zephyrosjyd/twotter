<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin"> Admin </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
      <CreateTwootPanel @add-twoot="addTwoot" />
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from './TwootItem';
import CreateTwootPanel from './CreaateTwootPanel';

export default {
  name: 'UserProfile',
  components: { TwootItem, CreateTwootPanel },
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: '_Zephyros',
        firstName: 'Luke',
        lastName: 'Ji',
        email: 'zephyrosjyd@gmail.com',
        isAdmin: true,
        twoots: [
          { id: 1, content: 'Twotter is amazing!' },
          { id: 2, content: "Don't forget to subscriber to The Earth is Square!" },
        ]
      }
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      console.log(`favorited twoot is #${id}`);
    },
    addTwoot(twoot) {
      this.user.twoots.unshift({
        id: this.user.twoots.length + 1, content: twoot
      });
    },
  },
  mounted() {
    this.followUser();
  }
}
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 20px;
  padding: 50px 5%;

  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;

    h1 {
      margin: 0;
    }

    .user-profile__admin-badge {
      background: purple;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 0 10px;
      margin-bottom: 20px;
    }
  }

  .user-profile__twoots-wrapper {
    display: grid;
    grid-gap: 10px
  }
}
</style>

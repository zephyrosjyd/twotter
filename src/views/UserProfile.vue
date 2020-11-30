<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ state.user.username }}</h1>
      <!-- <h2>{{ userId }}</h2> -->
      <div class="user-profile__admin-badge" v-if="state.user.isAdmin"> Admin </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ state.followers }}
      </div>
      <CreateTwootPanel @add-twoot="addTwoot" />
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem
        v-for="twoot in state.user.twoots"
        :key="twoot.id"
        :username="state.user.username"
        :twoot="twoot"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
import { reactive, computed } from 'vue';
import { useRouter } from 'vue-router';
import { users } from '../assets/users';
import TwootItem from '@/components/TwootItem';
import CreateTwootPanel from '@/components/CreaateTwootPanel';

export default {
  name: 'UserProfile',
  components: { TwootItem, CreateTwootPanel },
  setup() {
    const { value: { params }} = useRouter().currentRoute;
    const userId = computed(() => params.userId);

    // if (userId) fetchUserFromApi(userId)

    const state = reactive({
      followers: 0,
      user: users[userId.value - 1] || users[0]
    });

    function toggleFavorite(id) {
      console.log(`favorited twoot is #${id}`);
    }
    function addTwoot(twoot) {
      state.user.twoots.unshift({
        id: state.user.twoots.length + 1, content: twoot
      });
    }

    return {
      state,
      toggleFavorite,
      addTwoot,
      userId,
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.state.user.username} has gained a follower!`);
      }
    }
  },
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

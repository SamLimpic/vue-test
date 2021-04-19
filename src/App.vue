<template>
  <div class="text-center container">
    <div class="row">
      <div class="col">
        <img alt="Vue logo" src="./assets/logo.png" />
        <h1>Welcome to {{ state.myName }}'s Vue.js App</h1>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <form @submit.prevent="addBot">
          <p>{{ state.newBotName }}</p>
          <!-- NOTE v-model is 2 way data binding: it is pulled from and alters values from the state -->
          <input type="text" placeholder="Name..." v-model="state.newBotName" />
          <button type="submit" class="btn btn-dark mx-2">TO THE PIT!</button>
        </form>
      </div>
    </div>
    <div class="row justify-content-around mt-5">
      <!-- NOTE v-for iterates over a collection and requires a unique "key" that has to be a propery of the object and unique to each object
      It will repeat the element it is on -->
      <div
        class="col-md-3 border border-dark m-3 p-3"
        v-for="bot in state.enemies"
        :key="bot.name"
      >
        <!-- NOTE Binding an attribute makes the string of an attribute into js -->
        <img :src="bot.imgUrl" alt="" />
        <h1
          :class="{
            'text-success': bot.health > 66,
            'text-warning': bot.health <= 66 && bot.health > 0,
            'text-danger': bot.health <= 33,
            'text-dark': bot.health == 0,
          }"
        >
          {{ bot.name }} : {{ bot.health }}
        </h1>
        <!-- NOTE v-if, v-else-if, and v-else can be used on sibling elements as an if-statement shorthand -->
        <h2 v-if="bot.health === 0">GAME OVER!</h2>
        <h2 v-else-if="bot.health <= 33">ALMOST THERE!</h2>
        <h2 v-else-if="bot.health <= 66">KEEP GOING!</h2>
        <h2 v-else>START!</h2>
        <button
          type="button"
          class="btn btn-primary m-1"
          @click="clickMe(bot, 1)"
          :disabled="bot.health == 0"
        >
          Slap
        </button>
        <button
          type="button"
          class="btn btn-warning m-1"
          @click="clickMe(bot, 5)"
          :disabled="bot.health < 5"
        >
          Punch
        </button>
        <button
          type="button"
          class="btn btn-danger m-1"
          @click="clickMe(bot, 10)"
          :disabled="bot.health < 10"
        >
          Kick
        </button>
        <div class="row">
          <div class="col">
            <button
              type="button"
              class="btn btn-lg btn-success m-1"
              @click="bot.health = 100"
            >
              Reset
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";
// NOTE Every single script will need export default object

export default {
  // NOTE Whatever returns from the setup is accessible in the template, must always be an object
  setup() {
    // NOTE This is local state only, holds data that is unique to this component
    const state = reactive({
      myName: "Sam",
      health: 100,
      newBotName: "",
      enemies: [
        {
          name: "JSON",
          imgUrl: "https://robohash.org/JSON",
          health: 100,
        },
        {
          name: "R2D2",
          imgUrl: "https://robohash.org/R2D2",
          health: 100,
        },
        {
          name: "C3PO",
          imgUrl: "https://robohash.org/C3PO",
          health: 100,
        },
      ],
    });

    return {
      state,
      clickMe(bot, val) {
        bot.health -= val;
        bot.health = bot.health > 0 ? bot.health : 0;
      },
      addBot() {
        state.enemies.push({
          name: state.newBotName,
          imgUrl: "https://robohash.org/" + state.newBotName,
          health: 100,
        });
      },
    };
  },
};
</script>

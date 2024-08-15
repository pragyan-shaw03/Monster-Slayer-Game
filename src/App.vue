<template>
  <div>
    <header>
      <h1>Monster Slayer</h1>
    </header>
    <div class="container">
      <section>
        <h2>Monster</h2>
        <div class="monsterHealth" :style="{width: monsterHealth+'%'}"></div>
      </section>
      <section>
        <h2>Player</h2>
        <div class="playerHealth" :style="{width: playerHealth+'%'}"></div>
      </section>
      <div class="attacks" v-if="!finished">
        <button @click="attack">Attack</button>
        <button @click="specialAttack">Special Attack</button>
        <button @click="heal">Heal</button>
        <button @click="surrender">Surrender</button>
      </div>
      <section v-else><h1>{{winner}} Won</h1>
      <button @click="restart" style="width: 10rem;">Restart</button></section>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      finished: false,
      monsterHealth: 100,
      playerHealth: 100,
    };
  },
  computed: {
    winner() {
      if (this.monsterHealth > this.playerHealth) {
        return 'Monster ';
      } else {
        return 'Player ';
      }
    }
  },
  watch: {
    monsterHealth() {
      if (this.monsterHealth < 0) {
        this.monsterHealth = 0;
        this.finished = true;
      }
      if (this.monsterHealth > 100) {
        this.monsterHealth = 100;
      }
    },
    playerHealth() {
      if (this.playerHealth < 0) {
        this.playerHealth = 0;
        this.finished = true;
      }
      if (this.playerHealth > 100) {
        this.playerHealth = 100;
      }
    }
  },
  methods: {
    restart() {
      this.monsterHealth = 100;
      this.playerHealth = 100;
      this.finished = false;
    },
    attack() {
      this.monsterHealth -= Math.floor(Math.random()*(10-5) + 5);
      this.monsterAttack();
    },
    specialAttack() {
      this.playerHealth -= Math.floor(Math.random()*(10-5) + 5);
      this.monsterHealth -= Math.floor(Math.random()*(20-10) + 10);
      this.monsterAttack();
    },
    heal() {
      this.playerHealth += Math.floor(Math.random()*(20-10) + 10);
      this.monsterAttack();
    },
    surrender() {
      this.playerHealth = 0;
      this.finished = true;
    },
    monsterAttack() {
      let ind = Math.floor(Math.random()*3 + 1);
      if (ind == 1) {
        this.playerHealth -= Math.floor(Math.random()*(10-5) + 5);
      } else if (ind === 2) {
        this.playerHealth -= Math.floor(Math.random()*(20-10) + 10);
        this.monsterHealth -= Math.floor(Math.random()*(10-5) + 5);
      } else {
        this.monsterHealth += Math.floor(Math.random()*(20-10) + 10);
      }
    },
  }
};
</script>

<style scoped>
* {
  box-sizing: border-box;
  font-family: "Jost", sans-serif;
}
header {
  background-color:rgb(205, 3, 3);
  padding: 0.5rem;
}
h1 {
  text-align: center;
}
.container {
  width: 300px;
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}
section {
  width: 100%;
  text-align: center;
  border-radius: 1rem;
  box-shadow: 1px 1px 20px black;
  padding: 1rem;
  margin: 0.5rem;
}
.monsterHealth {
  height: 2rem;
  width: 100%;
  background-color: green;
}
.playerHealth {
  height: 2rem;
  width: 100%;
  background-color: green;
}
.attacks {
  display: flex;
  flex-direction: column;
  align-items: center;
}
button {
  height: 3rem;
  font-size: 1rem;
  width: 100%;
  margin: 0.5rem;
  border-radius: 1rem;
  color: white;
  background-color: brown;
}
button:hover {
  cursor: pointer;
  background-color: red;
}
button:active {
  background-color: brown;
}
</style>
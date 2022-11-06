<template>
  <!-- dialog S -->
  <dialog-component
    :title="diolog"
    v-if="isDialog"
    :result="score"
  ></dialog-component>
  <!-- dialog E -->
  <!-- score S -->
  <div class="containers score-container">
    <img src="./assets/pokeball.png" alt="pokemon ball" />
    <p>{{ score }}/10</p>
  </div>
  <!-- score E -->

  <div class="containers left-container" @click="toggleDisplay">
    <p class="option1" ref="option1">404</p>
  </div>
  <div class="containers middle-container">
    <!-- result S -->
    <p class="result" ref="name">?</p>
    <!-- result E -->

    <!-- poke-card S -->
    <div class="poke-card" :class="{ animation: this.isHidden }">
      <!-- Front of the Card  S -->
      <div class="poke-card__front">
        <img
          src="https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2Fwww.webdesignhot.com%2Fwp-content%2Fuploads%2F2014%2F08%2FSun-Rays-Blue-Background-Vector-Illustration.jpg&f=1&nofb=1&ipt=fa2a26058233efc5ce76906cbd437c67f94763554f58705dbd14d4b0a5ee75e4&ipo=images"
          alt=""
          class="poke-card__background-img"
        />
        <img
          src="./assets/pokemon-logo.png"
          alt="pokemon text logo"
          class="poke-card__logo-img1 poke-card__logo"
        />
        <img
          class="poke-card__img poke-card__img-front hidden"
          :src="`https://img.pokemondb.net/sprites/home/shiny/${currentPokemon.name}.png`"
          alt=""
        />
        <img
          src="./assets/pokemon-logo.png"
          alt="pokemon text logo"
          class="poke-card__logo-img2 poke-card__logo"
        />
      </div>
      <!-- Front of the Card  E -->

      <!-- Back of the Card  S -->
      <div class="poke-card__back">
        <div class="poke-card__effect"></div>
        <img
          src="https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2Fwww.webdesignhot.com%2Fwp-content%2Fuploads%2F2014%2F08%2FSun-Rays-Blue-Background-Vector-Illustration.jpg&f=1&nofb=1&ipt=fa2a26058233efc5ce76906cbd437c67f94763554f58705dbd14d4b0a5ee75e4&ipo=images"
          alt=""
          class="poke-card__background-img"
        />
        <img
          src="./assets/pokemon-logo.png"
          alt="pokemon text logo"
          class="poke-card__logo-img1 poke-card__logo"
        />
        <img
          class="poke-card__img poke-card__img-back"
          :src="`https://img.pokemondb.net/sprites/home/shiny/${currentPokemon.name}.png`"
          alt=""
        />
        <img
          src="./assets/pokemon-logo.png"
          alt="pokemon text logo"
          class="poke-card__logo-img2 poke-card__logo"
        />
      </div>
      <!-- Back of the Card  E -->
    </div>
    <!-- poke-card E -->
  </div>
  <div class="containers right-container" @click="toggleDisplay">
    <p class="option2" ref="option2">404</p>
  </div>
</template>

<script>
import DialogComponent from "./components/DialogComponent.vue";
export default {
  components: { DialogComponent },
  data() {
    return {
      round: 0,
      score: 0,
      isDialog: false,
      diolog: "Refresh to restart",
      currentPokemon: "",
      data: [],
      isHidden: false,
    };
  },
  methods: {
    fetchPokemonsFromAPI() {
      // https://img.pokemondb.net/sprites/black-white/anim/normal/bulbasaur.gif
      // https://pokeapi.co/api/v2/pokemon/?limit=1000
      axios.get("https://pokeapi.co/api/v2/pokemon/?limit=905").then((res) => {
        this.data = res.data.results;
        // console.log(res);
        // console.log(this.data);
        this.setCurrentPokemon();
      });
    },
    setCurrentPokemon() {
      // const randomNum = Math.floor(Math.random() * 906);
      // this.currentPokemon = this.data[randomNum];
      this.currentPokemon = this.pickRandomPokemon();

      if (this.isHidden) {
        this.isHidden = false;
      }
      this.setRandomOption();
    },
    pickRandomPokemon() {
      const randomNum = Math.floor(Math.random() * 906);
      const randomPokemon = this.data[randomNum];
      if (
        randomPokemon == "" ||
        randomPokemon == null ||
        randomPokemon == undefined
      ) {
        this.pickRandomPokemon();
      } else {
        return randomPokemon;
      }
    },
    setRandomOption() {
      const randomPokemon = this.pickRandomPokemon();
      const randomIndex = Math.floor(Math.random() * 2);

      const firstOption = this.$refs.option1;
      const secondOption = this.$refs.option2;

      if (randomIndex === 1) {
        firstOption.textContent = this.currentPokemon.name;
        secondOption.textContent = randomPokemon.name;
      } else {
        secondOption.textContent = this.currentPokemon.name;
        firstOption.textContent = randomPokemon.name;
      }
    },
    toggleDisplay(event) {
      this.isHidden = !this.isHidden;
      this.$refs.name.textContent = this.currentPokemon.name;
      this.checkAnswer(event);
      setTimeout(() => {
        this.isHidden = !this.isHidden;
        this.$refs.name.textContent = "?";
        this.setCurrentPokemon();
      }, 2500);
    },
    checkAnswer(event) {
      const answer = event.currentTarget.children[0].textContent;
      setTimeout(() => {
        document.querySelector("body").classList.remove("won");
        document.querySelector("body").classList.remove("lost");
        // this.isDialog = false;
        // this.diolog = "";
      }, 2500);

      if (this.currentPokemon.name == answer) {
        document.querySelector("body").classList.add("won");
        if (this.round >= 10) {
          this.isDialog = true;
        } else {
          this.round++;
          this.score++;
        }
        // this.isDialog = true;
        // this.diolog = "You are right!";
      } else {
        document.querySelector("body").classList.add("lost");
        if (this.round >= 10) {
          this.isDialog = true;
        } else {
          this.round++;
        }
        // this.isDialog = true;
        // this.diolog = "You are wrong!";
      }
    },
  },
  created() {
    // this.setRandomOption();
    this.fetchPokemonsFromAPI();
  },
  mounted() {},
};
</script>

<style>
.containers {
  /* border: 1px solid white; */

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
p {
  text-align: center;
  margin: 0;
  /* color: rgb(231, 185, 1);
  color: rgb(255, 255, 255); */
  color: rgb(6, 106, 194);
}

/* ------------------------------------------ */
.score-container {
  padding: 20px;
  grid-area: score-container;
  margin-top: 10px;

  font-size: 1.2rem;
  padding: 0 20px;

  justify-content: end;
  flex-direction: row;
}
.score-container img {
  height: 40px;
}
.score-container p {
  font-weight: 400;
  color: white;
}

/* ------------------------------------------- */
.left-container {
  grid-area: left-container;

  margin-top: 72px;
  margin-left: 30px;
  height: 450px;
  width: 100%;
  max-width: 300px;

  background-color: rgb(255, 255, 255);
  justify-self: center;
  align-self: center;
}
.left-container p {
  margin: 0;
  flex-wrap: wrap;
  color: rgb(234, 0, 17);
}

/* -------------------------------------------- */
.middle-container {
  padding: 20px;
  grid-area: middle-container;
  font-family: "Irish Grover", cursive;
  perspective: 800px;
}

/* -------------------------------------------- */
.right-container {
  grid-area: right-container;

  margin-top: 72px;
  margin-right: 30px;
  height: 450px;
  width: 100%;
  max-width: 300px;

  /* background-color: rgb(234, 0, 17); */
  background-color: rgb(255, 255, 255);

  justify-self: center;
  align-self: center;
}
.right-container p {
  margin: 0;
  flex-wrap: wrap;
  /* color: white; */
  color: rgb(234, 0, 17);
}

/* ------------------------------------------- */
.poke-card {
  position: relative;
  height: 430px;
  width: 300px;

  background-color: black;
  border-radius: 10px;

  transition: transform 150ms;
  transform-style: preserve-3d;
}

.poke-card.animation {
  cursor: pointer;
  transform: rotateY(180deg);
  transition: transform 1500ms;
}

.poke-card__front,
.poke-card__back {
  height: 100%;
  width: calc(100% - 16px);

  border: 8px solid rgb(6, 106, 194);
  background-color: rgb(0, 57, 170);
  border-radius: 10px;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.486);
  backface-visibility: hidden;
}

.poke-card__back {
  top: 0;
  position: absolute;
  transform: rotateY(180deg);
}

.poke-card__effect {
  z-index: 2;
  position: absolute;
  background-image: url("./assets/sparkles.gif");
  background-position: center;
  background-size: 180%;
  mix-blend-mode: color-dodge;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.poke-card__background-img {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: saturate(200%) grayscale(50%);
}

.poke-card__logo {
  z-index: 1;
  width: 250px;
}
.poke-card__logo-img2 {
  rotate: 180deg;
}

.poke-card__img {
  -webkit-user-drag: none;
  user-select: none;
  z-index: 2;
  height: 240px;
}

.hidden {
  filter: contrast(0%) brightness(50%);
}

.won {
  background-color: rgb(38, 182, 38);
  background-image: url("./assets/right-sign.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.lost {
  background-color: rgb(224, 43, 15);
  background-image: url("./assets/wrong-sign.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

/* Media Query S ----------------------------- */
@media screen and (max-width: 900px) {
  .left-container {
    margin-left: 0px;
    height: 100%;
    max-width: none;

    border-radius: unset;
  }
  .right-container {
    margin-right: 0px;
    height: 100%;
    max-width: none;

    border-radius: unset;
  }
}

@media screen and (max-width: 600px) {
}
/* Media Query E ----------------------------- */
</style>

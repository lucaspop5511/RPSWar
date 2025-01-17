<template>
    <div class="game-screen"
    :style="{
      backgroundImage: `url(${require('@/assets/Background.png')})`,
      backgroundSize: 'cover',
      backgroundPosition: 'center',
      backgroundRepeat: 'no-repeat'
    }"
    >
        <img src="@/assets/TitleLogo.png" alt="RPS War" class="title-logo" />
        <h2 v-if="!result">Choose a weapon!</h2>
  
        <div class="choices">

            <div class="choises-frame" v-if="!result">
                <button v-for="weapon in weapons" :key="weapon" @click="playGame(weapon)">
                    <img :src="require(`@/assets/${weapon}.png`)" :alt="weapon" />
                </button>
            </div>
    
            <div class="result-frame" v-else>
                <button><img :src="require(`@/assets/${humanChoice}.png`)" :alt="humanChoice" /></button>
                <button class="result-button">{{ resultMessage }}</button>
                <button><img :src="require(`@/assets/${computerChoice}.png`)" :alt="computerChoice" /></button>
            </div>
            
        </div>

        <div class="scoreboard">
            <p>{{ humanScore }}</p>
            <p>{{ computerScore }}</p>
        </div>
  
        <button class="new-battle" @click="resetGame" ref="newBattleButton"><p>New Battle</p></button>
  
        <div class="footer">
            <img src="@/assets/human.png" alt="Human" class="footer-image" />
            <img src="@/assets/computer.png" alt="Computer" class="footer-image" />
        </div>

    </div>
  </template>  
  
  <script>
  export default {
    data() {
      return {
        weapons: ['rock', 'paper', 'scissors'],
        humanChoice: null,
        computerChoice: null,
        result: null,
        humanScore: 0,
        computerScore: 0,
      };
    },
    computed: {
      resultMessage() {
        if (this.result === 'win') return 'You Win!';
        if (this.result === 'lose') return 'You Lose!';
        return 'Tie!';
      },
    },
    methods: {
    playGame(weapon) {
        this.humanChoice = weapon;
        this.computerChoice = this.weapons[Math.floor(Math.random() * 3)];
        this.calculateResult();
        this.showNewBattleButton();
    },
    calculateResult() {
        const winConditions = {
        rock: 'scissors',
        scissors: 'paper',
        paper: 'rock',
        };

        if (this.humanChoice === this.computerChoice) {
        this.result = 'tie';
        } else if (winConditions[this.humanChoice] === this.computerChoice) {
        this.result = 'win';
        this.humanScore++;
        } else {
        this.result = 'lose';
        this.computerScore++;
        }
    },
    resetGame() {
        const resultFrame = document.querySelector('.result-frame');
        const newBattleButton = this.$refs.newBattleButton;

        if (resultFrame) {
        resultFrame.classList.add('reverse');
        setTimeout(() => {
            resultFrame.classList.remove('reverse');
            this.humanChoice = null;
            this.computerChoice = null;
            this.result = null;

            // New Battle button out
            if (newBattleButton) {
            newBattleButton.classList.remove('visible');
            newBattleButton.classList.add('hidden'); // slide-down animation

            // reset button state after the animation completes
            newBattleButton.addEventListener(
                'animationend',
                () => {
                newBattleButton.classList.remove('hidden');
                },
                { once: true }
            );
            }
        }, 500); // match the resultFrame animation
        }
    },
    showNewBattleButton() {
        const newBattleButton = this.$refs.newBattleButton;

        // Animate the button in
        if (newBattleButton) {
        newBattleButton.classList.remove('hidden');
        newBattleButton.classList.add('visible'); // also trigger slide-up animation
        }
    },
    },
  };
  </script>
  
  <style src="../styles/GameScreen.css"></style>
  <style src="../styles/ResultAnimations.css"></style>
  
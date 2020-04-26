<template>
    <div id="app" class="app-container">
        <section class="score-section">
            <div class="score-board">
                <div class="score-board__title">Rock <br> Paper <br> Scissors</div>
                <div class="score-board__box">
                    <span class="score-title">score</span>
                    <span class="score-value">{{ score }}</span>
                </div>
            </div>
        </section>
        <section class="game-section">
            <div v-if="selected == null" class="game">
                <GamePiece selection="rock" @click="choose('rock')" class="game__selection rock"/>
                <GamePiece selection="paper" @click="choose('paper')" class="game__selection paper"/>
                <GamePiece selection="scissors" @click="choose('scissors')" class="game__selection scissors"/>
            </div>
            <div v-else class="play-round">
                <div class="result">
                    <div class="result__group">
                        <GamePiece :selection="this.selected"></GamePiece>
                        <span class="badge__label">You Picked</span>
                    </div>
                    <div class="result__group">
                        <GamePiece :selection="this.computerSelection"></GamePiece>
                        <span class="badge__label">The house picked</span>
                    </div>
                </div>
                <div class="play-again">
                    <span>{{ roundResult }}</span>
                    <button @click.prevent="playAgain">Play again</button>
                </div>
            </div>
        </section>
        <section class="rules-section">
            <button @click.prevent="showModal" class="rules-btn">Rules</button>
        </section>
        <div class="modal" v-if="isModalVisible">
            <div class="modal__wrapper">
                <h1>Rules</h1>
                <img src="./assets/images/image-rules.svg" alt="Basic Rules">
                <button @click.prevent="hideModal">
                    <img src="./assets/images/icon-close.svg" alt="">
                </button>
            </div>
        </div>
    </div>
</template>

<script>
    import GamePiece from "./components/GamePiece";
    const choices = ['rock', 'paper', 'scissors'];

    export default {
        name: 'App',
        components: {
            GamePiece,
        },
        data() {
            return {
                score: 0,
                selected: null,
                isModalVisible: false,
                computerSelection: null,
                roundResult: '',
            };
        },
        methods: {
            choose(selection) {
                this.roundResult = '';
                this.selected = selection;
                this.computerSelection = choices[Math.ceil(Math.random() * 3) - 1];

                if (this.selected === this.computerSelection) {
                    this.roundResult = 'It\'s a tie';
                    return;
                }
                const selectedIndex = choices.findIndex((predicate) => {
                    return selection === predicate
                });
                const winningIndex = (selectedIndex - 1 + choices.length) % choices.length;
                if (choices[winningIndex] === this.computerSelection) {
                    this.roundResult = 'You Win';
                    this.score++;
                    return;
                }

                this.roundResult = 'You Lose';
                this.score--;
            },
            playAgain() {
                this.selected = null;
            },
            showModal() {
                this.isModalVisible = true;
            },
            hideModal() {
                this.isModalVisible = false;
            },
        }
    }
</script>

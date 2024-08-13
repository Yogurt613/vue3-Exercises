<!----

1.請輸入挑戰者名稱input的部分用v-model,@keyup.enter="confirm"

2.接著按下確認@click="confirm"

3.v-if="!game.started" 如果遊戲未開始會停在這個區塊<h4>挑戰者{{challengerName}}</h4>

4.開始遊戲@click="Start game"

5.反之v-else遊戲開始 接著猜數字要說請猜min 到max值,input輸入框用v-bind綁定

6.接著提交@click="submit"

7.回復{{reply}}如果回覆v-if="reply"是有值且不是空值就顯示{{reply}}的內容

8.如果v-if="Complete.challenge"恭喜你完成挑戰！將顯示總挑戰次數：{{ totalTimes }}的內容---->
<template>

    <div id="app" class="container mt-4">

        <div v-if="currentScreen === 'home'" class="text-center">

            <div class="card text-center">

                <div class="card-body">

                    <h4>請輸入挑戰者名稱</h4>

                    <input type="text" v-model="challengerName" @keyup.enter="confirmChallengerName"
                        class="form-control mb-3" />

                    <button class="btn btn-primary" @click="confirmChallengerName">確認</button>

                </div>

            </div>

        </div>

        <div v-if="currentScreen === 'game'">

            <div class="card text-center">

                <div class="card-body">

                    <div v-if="!gameStarted">

                        <h4>挑戰者：{{ challengerName }}</h4>

                        <button class="btn btn-primary" @click="startGame">開始遊戲</button>

                    </div>

                    <div v-else>

                        <p class="card-text">猜一個 {{ min }} 到 {{ max }} 之間的數字：</p>

                        <input type="number" v-model.number="guess" @keyup.enter="submit" class="form-control mb-3" />

                        <button class="btn btn-primary" @click="submit">提交</button>

                        <p class="mt-3" v-if="reply">{{ reply }}</p>

                        <p class="mt-3" v-if="completeChallenge">恭喜你完成挑戰！總挑戰次數：{{ totalTimes }}</p>

                    </div>

                </div>

            </div>

        </div>

        <div v-if="currentScreen === 'topTwenty'">

            <div class="card text-center">

                <div class="card-body">

                    <h4>前二十名挑戰者</h4>

                    <ol>

                        <li v-for="(record, index) in topTwentyRecords" :key="index">

                            {{ record.name }} {{ record.frequency }} 次

                        </li>

                    </ol>

                    <button class="btn btn-primary mt-3" @click="goBackToGame">返回遊戲</button>

                </div>

            </div>

        </div>

    </div>

</template>

<script>

export default {

    data() {

        return {

            currentScreen: 'home', // 當前畫面為首頁

            gameStarted: false,

            challengerName: '',

            secretNumber: 0, // 初始化為0，稍後隨機生成

            guess: '', // 初始化為空字串

            reply: '',

            completeChallenge: false,

            totalTimes: 0,

            min: 1, // 最小範圍

            max: 30, // 最大範圍

            records: [

                { name: '玩家1', frequency: 10 },

                { name: '玩家2', frequency: 15 },

                { name: '玩家3', frequency: 13 },

                // 放入假資料

            ],

            topTwentyRecords: []

        };

    },

    methods: {

        confirmChallengerName() {

            if (this.challengerName.trim() !== '') {

                this.currentScreen = 'game';

                this.resetGame(); // 重置遊戲狀態

            } else {

                alert('請輸入挑戰者名稱');

            }

        },

        startGame() {

            this.gameStarted = true;

            this.secretNumber = Math.floor(Math.random() * (this.max - this.min + 1)) + this.min; // 隨機生成 1 到 30 之間的數字

        },

        submit() {

            // 檢查輸入的數字是否在範圍內

            if (this.guess < this.min || this.guess > this.max) {

                this.reply = `請輸入 ${this.min} 到 ${this.max} 之間的數字。`;

                return; // 不繼續執行後續邏輯

            }

            this.totalTimes++;

            if (this.guess === this.secretNumber) {

                this.reply = '恭喜你猜對了！';

                this.completeChallenge = true;

                this.addRecord(); // 添加記錄

                setTimeout(() => {

                    this.currentScreen = 'topTwenty'; // 顯示前二十名挑戰者畫面

                }, 2000); // 2秒後顯示排行榜畫面

                this.guess = ''; // 清空輸入的數字

            } else if (this.guess > this.secretNumber) {

                this.reply = `請猜小一點。範圍從 ${this.min} 到 ${this.guess - 1}。`;

                this.max = this.guess - 1; // 調整最大範圍

            } else if (this.guess < this.secretNumber) {

                this.reply = `請猜大一點。範圍從 ${this.guess + 1} 到 ${this.max}。`;

                this.min = this.guess + 1; // 調整最小範圍

            }

        },

        addRecord() {

            this.records.push({ name: this.challengerName, frequency: this.totalTimes });

            this.records.sort((a, b) => a.frequency - b.frequency);

            this.topTwentyRecords = this.records.slice(0, 20);

        },

        goBackToGame() {

            this.currentScreen = 'home';

            this.challengerName = '';

            this.guess = ''; // 清空數字輸入框

        },

        resetGame() {

            this.guess = ''; // 清空數字輸入框

            this.reply = '';

            this.completeChallenge = false;

            this.totalTimes = 0;

            this.min = 1;

            this.max = 30;

        }

    }

};

</script>

<style>
#app {

    text-align: center;

}
</style>
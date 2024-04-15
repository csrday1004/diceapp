<template >

  <div style="margin:auto; text-align: center; max-width:700px">
    <div>
      <div>
        <div v-if="step == 1" style="text-align: center; height:150px;">
          <img src="./assets/diceImg/dicegif.gif" style="width:150px;" alt="다이스이미지" @click="한번만던지기">
        </div>

        <div v-if="step == 2" style="text-align: center; height:150px;">
          <img :src="require(`@/assets/diceImg/${주사위결정[랜덤숫자].img}`)" style="width:90px; margin: 30px;" @click="한번만던지기">
        </div>

        <!-- 주사위리스트 -->
        <div
          style="overflow:auto; height: 200px; text-align: left; margin-top:20px; word-wrap: break-word; margin: 10px; border: none; background-color:whitesmoke">
          <span v-for="(a) in 기록" :key="a" style="margin:10px; font-size:30px;">
            <img :src="require(`@/assets/diceImg/${주사위결정[a - 1].img}`)" style="width:30px;" />
          </span>
        </div>

        <!-- 결과 정보 -->
        <div style="margin-bottom:10px">주사위 ( {{ 남은주사위 }} / {{ 이만큼던지기로함 }} 개)</div>
        <div>주사위 눈의 총 합계는? ({{ 계 }})</div>



        <div>
          <!-- 퀵버튼 -->
          <div class="quick" style="display:flex; width:auto;">
            <button type="button" @click="던질횟수++; btnstep=1; 이만큼던지기로함=던질횟수; 남은주사위=던질횟수-던진횟수;" class="btn btn-danger">+1개</button>
            <button type="button" @click="던질횟수 += 10; btnstep=1; 이만큼던지기로함=던질횟수; 남은주사위=던질횟수-던진횟수;" class="btn btn-warning">+10개</button>
            <button type="button" @click="던질횟수 = 100; btnstep=1; 이만큼던지기로함=던질횟수; 남은주사위=던질횟수-던진횟수;" class="btn btn-info">100개</button>
          </div>

          <!-- 입력 -->
          <div style="margin:10px;">
            <div class="input-group input-group-lg">
              <span class="input-group-text" id="inputGroup-sizing-lg">던질 횟수</span>
              <input type="text" style="margin-right:10px;" v-model="던질횟수" class="form-control"
                aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg" placeholder="100 이하 숫자">
            </div>
          </div>
          <!-- 버튼 -->
          <div style="display:flex">
            <button v-if="btnstep == 1" @click="주사위굴리기">수동</button>
            <button v-if="btnstep == 1" @click="오토굴리기">자동</button>
            <button v-if="btnstep == 2" @click="초기화">초기화</button>
          </div>

        </div>

      </div>
    </div>

  </div>

</template>

<script>

import Dices from './assets/dices.js'

export default {
  name: 'App',

  data() {
    return {
      btnstep: 1,
      step: 1,
      랜덤숫자: 0,
      주사위결정: Dices,
      기록: [],
      던진횟수: 0,
      던질횟수: 1,
      계: 0,
      이만큼던지기로함: 1,
      남은주사위: 1,
      던졌니: false,
      증가폭10: 10,
    }
  },

  watch: {
    던질횟수(a) {
      if (a > 100) {
        alert("100번 넘게 던지고 싶으면 100만원 내야댐")
        this.던질횟수 = 1
        this.초기화()
      }
    }
  },

  methods: {

    한번만던지기() {
      if (this.던졌니 == false) {
        this.던졌니 = true
        this.step = 2
        this.랜덤숫자 = Math.floor((Math.random() * 6))
      }
      else {
        this.step = 1
        this.던졌니 = false
      }
    },

    오토굴리기() {
      for (let a = this.기록.length; a < this.던질횟수; a++) {
        this.주사위굴리기()
      }
      this.btnstep = 2
    },

    주사위굴리기() {
      if (this.던진횟수 < this.던질횟수) {
        this.이만큼던지기로함 = this.던질횟수
        this.랜덤숫자 = Math.floor((Math.random() * 6))
        this.step = 2
        this.던진횟수++
        this.남은주사위 = this.던질횟수 - this.던진횟수
        this.기록.unshift(this.랜덤숫자 + 1)
        this.계 += this.랜덤숫자 + 1
        if (this.던진횟수 == this.던질횟수) {
          this.btnstep = 2
        }
      }
      else {
        this.btnstep = 2
      }
    },

    초기화() {
      this.step = 1
      this.btnstep = 1
      this.던진횟수 = 0
      this.기록 = []
      this.던질횟수 = 1
      this.계 = 0
      this.이만큼던지기로함 = 1
      this.남은주사위 = 1
    }

  },
  components: {

  }
}

</script>

<style>
.quick button {
  height: auto;
  background-color: bisque;
  border: none;
  color: black;
}

button {
  width: 100%;
  height: 50px;
  background-color: bisque;
  border: none;
  font-size: 20px;
  margin: 10px;
  border-radius: 10px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>

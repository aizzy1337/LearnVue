<template>
  <div class="content">
    <buttom class="add-to-cart" @click="addToCart()">Add to Cart</buttom>
    <div class="top-row">
      <div class="top part">
        <div class="robot-name">
          {{ selectedRobot.head.title }}
          <span class="sale" v-if="selectedRobot.head.onSale">Sale!</span>
        </div>
        <img :src="selectedRobot.head.imageUrl" alt="head" />
        <button @click="selectPreviousHead()" class="prev-selector">&#9668;</button>
        <button @click="selectNextHead()" class="next-selector">&#9658;</button>
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <img v-bind:src="selectedRobot.leftArm.imageUrl" alt="left arm" />
        <button v-on:click="selectPreviousLeftArm()" class="prev-selector">&#9650;</button>
        <button v-on:click="selectNextLeftArm()" class="next-selector">&#9660;</button>
      </div>
      <div class="center part">
        <img v-bind:src="selectedRobot.torso.imageUrl" alt="torso" />
        <button v-on:click="selectPreviousTorso()" class="prev-selector">&#9668;</button>
        <button v-on:click="selectNextTorso()" class="next-selector">&#9658;</button>
      </div>
      <div class="right part">
        <img v-bind:src="selectedRobot.rightArm.imageUrl" alt="right arm" />
        <button v-on:click="selectPreviousRightArm()" class="prev-selector">&#9650;</button>
        <button v-on:click="selectNextRightArm()" class="next-selector">&#9660;</button>
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <img v-bind:src="selectedRobot.base.imageUrl" alt="base" />
        <button v-on:click="selectPreviousBase()" class="prev-selector">&#9668;</button>
        <button v-on:click="selectNextBase()" class="next-selector">&#9658;</button>
      </div>
    </div>
  </div>
  <div>
    <h1>Cart</h1>
    <table>
      <thread>
        <tr>
          <th>Robot</th>
          <th class="cost">Cost</th>
        </tr>
      </thread>
      <tbody>
        <tr v-for="(robot, index) in cart" :key="index">
          <td>{{ robot.head.title }}</td>
          <td class="cost">{{ toCurrency(robot.cost) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import { toCurrency } from '@/shared/formatters';
import parts from '../data/parts';

function getNextValidIndex(index, length) {
  let newIndex = index + 1;
  if (newIndex >= length) {
    newIndex = 0;
  }
  return newIndex;
}

function getPreviousValidIndex(index, length) {
  let newIndex = index - 1;
  if (newIndex < 0) {
    newIndex = length - 1;
  }
  return newIndex;
}

const availableParts = parts;
const selectHeadIndex = ref(0);
const selectLeftArmIndex = ref(0);
const selectRightArmIndex = ref(0);
const selectTorsoIndex = ref(0);
const selectBaseIndex = ref(0);
const cart = ref([]);

const selectedRobot = computed(() => ({
  head: availableParts.heads[selectHeadIndex.value],
  leftArm: availableParts.arms[selectLeftArmIndex.value],
  rightArm: availableParts.arms[selectRightArmIndex.value],
  torso: availableParts.torsos[selectTorsoIndex.value],
  base: availableParts.bases[selectBaseIndex.value],
}));

const addToCart = () => {
  const robot = selectedRobot.value;
  const cost = robot.head.cost +
    robot.leftArm.cost +
    robot.rightArm.cost +
    robot.torso.cost +
    robot.base.cost;
  cart.value.push({ ...robot, cost });
};

const selectNextHead = () => {
  selectHeadIndex.value =
    getNextValidIndex(selectHeadIndex.value, availableParts.heads.length);
};

const selectPreviousHead = () => {
  selectHeadIndex.value =
    getPreviousValidIndex(selectHeadIndex.value, availableParts.heads.length);
};

const selectNextLeftArm = () => {
  selectLeftArmIndex.value =
    getNextValidIndex(selectLeftArmIndex.value, availableParts.arms.length);
};

const selectPreviousLeftArm = () => {
  selectLeftArmIndex.value =
    getPreviousValidIndex(selectLeftArmIndex.value, availableParts.arms.length);
};

const selectNextRightArm = () => {
  selectRightArmIndex.value =
    getNextValidIndex(selectRightArmIndex.value, availableParts.arms.length);
};

const selectPreviousRightArm = () => {
  selectRightArmIndex.value =
    getPreviousValidIndex(selectRightArmIndex.value, availableParts.arms.length);
};

const selectNextTorso = () => {
  selectTorsoIndex.value =
    getNextValidIndex(selectTorsoIndex.value, availableParts.torsos.length);
};

const selectPreviousTorso = () => {
  selectTorsoIndex.value =
    getPreviousValidIndex(selectTorsoIndex.value, availableParts.torsos.length);
};

const selectNextBase = () => {
  selectBaseIndex.value =
    getNextValidIndex(selectBaseIndex.value, availableParts.bases.length);
};

const selectPreviousBase = () => {
  selectBaseIndex.value =
    getPreviousValidIndex(selectBaseIndex.value, availableParts.bases.length);
};
</script>

<style scoped>
.part {
  position: relative;
  width: 200px;
  height: 200px;
  border: 3px solid #aaa;
}

.part img {
  width: 200px;
}

.top-row {
  display: flex;
  justify-content: space-around;
}

.middle-row {
  display: flex;
  justify-content: center;
}

.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}

.top {
  border-bottom: none;
}

.left {
  border-right: none;
}

.right {
  border-left: none;
}

.left img {
  transform: rotate(-90deg);
}

.right img {
  transform: rotate(90deg);
}

.bottom {
  border-top: none;
}

.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 206px;
}

.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 206px;
}

.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}

.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 179px;
  height: 25px;
}

.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 179px;
  height: 25px;
}

.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 179px;
  height: 25px;
}

.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 179px;
  height: 25px;
}

.right .next-selector {
  right: -3px;
}

.robot-name {
  position: absolute;
  top: -25px;
  text-align: center;
  width: 100%;
}

.sale {
  color: red;
}

.content {
  position: relative;
}

.add-to-cart {
  position: absolute;
  right: 30px;
  width: 220px;
  padding: 3px;
  font-size: 16px;
}

td,
th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}

.cost {
  text-align: right;
}
</style>

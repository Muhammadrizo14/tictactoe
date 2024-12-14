<template>
  <div class="relative h-[600px] w-[600px] mx-auto">
    <div class="text-center flex items-center justify-center">
      <ConfettiExplosion :colors="['var(--yellow)', 'var(--red)', '#000', '#000']" v-if="winner"   :particleCount="250"  :duration="2500"/>

    </div>
    <div class="grid grid-cols-3 items-center " :class="winner&&'blur-sm'">
      <div class="h-[20vh] game__board" v-for="(node, i) in game" :key="i" @click="picking(i)">
        <div v-if="node === 'X'" class="flex items-center h-full w-full justify-center">
          <div class="p-3 h-[90%] bg-black rotate-[50deg] relative left-[7px]"></div>
          <div class="p-3 h-[90%] bg-black -rotate-[50deg] relative left-[-7px]"></div>
        </div>
        <div v-if="node==='O'" class="flex items-center h-full w-full justify-center ">
          <div class="rounded-full p-3 h-[90%] w-[90%] border-[20px] border-black">
          </div>
        </div>
      </div>
    </div>
    <div class="h-full w-full  z-50 absolute  top-0 left-0 flex items-center text-center justify-center"
         style="background: rgba(255, 255, 255, 0.5)" v-if="winner">
      <div class="blur-none">
        <h1 class="text-3xl pb-2">{{ winner }}</h1>
        <button @click="again()" class="bg-black text-white px-4 py-3 ">Play Again!</button>
      </div>
    </div>

    <div class="h-full w-full  z-50 absolute  top-0 left-0 flex items-center text-center justify-center"
         style="background: rgba(255, 255, 255, 0.5)" v-if="draw">
      <div class="blur-none">
        <h1 class="text-3xl pb-2">Draw</h1>
        <button @click="again()" class="bg-black text-white px-4 py-3 ">Play Again!</button>
      </div>
    </div>

  </div>
</template>

<script setup>
import ConfettiExplosion from "vue-confetti-explosion";

const timeTo = ref('X')
const winner = ref()
const draw = ref(false)
const game = ref(new Array(9).fill(0))


const again = () => {
  winner.value = null
  game.value.fill(0)
  draw.value = false
  timeTo.value = 'X'
}

const picking = async (node) => {
  if (game.value[node] === 0) {
    switch (timeTo.value) {
      case 'X':
        game.value[node] = 'X'
        const res = await check()
        if (res !== undefined) {
          winner.value = `${timeTo.value} won the game`

        }
        timeTo.value = 'O'

        break
      case 'O':
        game.value[node] = 'O'
        const res1 = await check()
        if (res1 !== undefined) {
          winner.value = `${timeTo.value} won the game`
        }
        timeTo.value = 'X'
        break;
    }
  }
}

const check = async () => {
  const wins = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  let isDraw = game.value.find((i)=> i === 0)
  if (isDraw === undefined) {
    draw.value = true
  }

  return wins.find((combo) =>
      combo.every((i) => game.value[i] === timeTo.value)
  );
}

</script>

<style scoped>

.game__board:nth-child(1), .game__board:nth-child(2), .game__board:nth-child(4), .game__board:nth-child(5), .game__board:nth-child(7), .game__board:nth-child(8){
  border-right: 4px solid black;
}
.game__board:nth-child(1), .game__board:nth-child(2), .game__board:nth-child(3), .game__board:nth-child(4), .game__board:nth-child(5), .game__board:nth-child(6) {
  border-bottom: 4px solid black;
}

.vert {
  border-left: 2px solid black;
  border-right: 2px solid black;
}
.hori {
  border-top: 2px solid black;
  border-bottom: 2px solid black;
}
</style>
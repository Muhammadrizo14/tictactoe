<template>
  <div class="grid grid-cols-3 gap-2 h-[600px] w-[600px] items-center mx-auto">
    <div class="border h-[20vh]" v-for="(node, i) in game" :key="i" @click="picking(i)">
      <div v-if="node === 'X'" class="flex items-center h-full w-full justify-center">
        <div class="p-3 h-[90%] bg-black rotate-45"></div>
        <div class="p-3 h-[90%] bg-black -rotate-45"></div>
      </div>
      <div v-if="node==='O'" class="flex items-center h-full w-full justify-center ">
        <div class="rounded-full p-3 h-[90%] w-[90%] border-8 border-black">

        </div>
      </div>
    </div>
    {{ game }}
    <div class="h-screen w-screen bg-white absolute top-0 left-0 flex items-center  justify-center" v-if="winner">
      <h1 class="text-3xl">{{winner}}</h1>
      <button @click="again()">play again!</button>
    </div>
  </div>
</template>

<script setup>
const timeTo = ref('X')
const winner = ref()
const game = ref(new Array(9).fill(0))


const again = () => {
  winner.value = null
  game.value.fill(0)
  timeTo.value = 'X'
}

const picking = async (node) => {
  if (game.value[node] === 0) {
    switch (timeTo.value) {
      case 'X':
        game.value[node] = 'X'
        const res = await check()
        if (res !== undefined) {
          winner.value  = `${timeTo.value} won the game`

        }
        timeTo.value = 'O'

        break
      case 'O':
        game.value[node] = 'O'
        const res1 = await check()
        if (res1 !== undefined) {
          winner.value  = `${timeTo.value} won the game`
        }
        timeTo.value = 'X'
        break;
    }
  }
}

const check = async () => {
  let callcount = 1
  callcount++
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

  return wins.find((combo) =>
      combo.every((i) => game.value[i] === timeTo.value)
  );


}

</script>

<style lang="scss" scoped>

</style>
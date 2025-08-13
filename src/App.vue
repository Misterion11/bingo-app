<template>
  <main class="min-h-screen bg-gray-100 flex flex-col items-center justify-center p-8">
    <h1 class="text-3xl sm:text-4xl font-bold text-center mb-12">
      Bingo Ali
    </h1>

    <div
      class="grid gap-4 w-[90%] max-w-4xl h-[500px]"
      :style="{ 
        gridTemplateColumns: `repeat(${cols}, minmax(0, 1fr))`,
        gridTemplateRows: `repeat(${rows}, 1fr)` 
      }"
    >
      <button
        v-for="(cell, index) in bingoGrid"
        :key="index"
        type="button"
        @click="toggle(index)"
        class="bg-white text-center border rounded-3xl shadow-lg text-base sm:text-xl font-semibold leading-snug transition-all duration-200 select-none cursor-pointer p-10 sm:p-12 appearance-none touch-manipulation outline-none focus:outline-none active:bg-transparent"
        :class="selected.has(index)
          ? 'bg-green-100 border-green-500 shadow-2xl scale-105'
          : 'border-gray-300 hover:shadow-2xl hover:scale-105'"
      >
        <span :class="selected.has(index) ? 'line-through text-gray-700' : ''">
          {{ cell }}
        </span>
      </button>
    </div>
  </main>
</template>

<script>
export default {
  name: "BingoGrid",
  data() {
    return {
      size: 5,
      cols: 3,
      rows: 5,
      selected: new Set(),
      items: [
        "j'ai faim !", "Je m'ennuie", "On peut s'arrêter aux toilletes", "Metz c'est vraiment nul", "on arrive quand ?", "On aurait dû aller à [insérer ville]", "J'ai mal aux jambes", "J'ai chauuuud / Y a pas de ventilo", "Sur twitter pendant qu'on mange",
        "Mon pc me manque", "Blague sur Zetsu", "Blague sur Renarbre", "Critique sur la conduite", "Hate de voir Srayzz", "Blague sur perso d'Ow"
      ]
    }
  },
  computed: {
    bingoGrid() {
      return this.items.slice(0, this.size * this.size)
    }
  },
  mounted() {
    const setCols = () => { 
      this.cols = window.innerWidth < 640 ? 3 : this.size 
      this.rows = Math.ceil(this.bingoGrid.length / this.cols)
    }
    setCols()
    window.addEventListener('resize', setCols)
    this._setCols = setCols

    const saved = localStorage.getItem("bingo-selected")
    if (saved) {
      this.selected = new Set(JSON.parse(saved))
    }
  },
  beforeUnmount() {
    window.removeEventListener('resize', this._setCols)
  },
  methods: {
    toggle(index) {
      if (this.selected.has(index)) {
        this.selected.delete(index)
      } else {
        this.selected.add(index)
      }
      this.selected = new Set(this.selected)
      localStorage.setItem("bingo-selected", JSON.stringify([...this.selected]))
    }
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: sans-serif;
}
</style>

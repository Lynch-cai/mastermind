<template>
    <div class="flex gap-2">
        <div class="space-y-2 border-r-2 border-white pr-2">
            <div class="circle border-blue-600 border-2 bg-blue-400 text-white" @click="selectColor('blue')">
                <span v-if="selectedColor === 'blue'">
                    <font-awesome-icon :icon="['fas', 'check']" size="2xl" />
                </span>
            </div>
            <div class="circle border-green-600 border-2 bg-green-400 text-white" @click="selectColor('green')">
                <span v-if="selectedColor === 'green'">
                    <font-awesome-icon :icon="['fas', 'check']" size="2xl" />
                </span>
            </div>
            <div class="circle border-red-600 border-2 bg-red-400 text-white" @click="selectColor('red')">
                <span v-if="selectedColor === 'red'">
                    <font-awesome-icon :icon="['fas', 'check']" size="2xl" />
                </span>
            </div>
            <div class="circle border-yellow-600 border-2 bg-yellow-400 text-white" @click="selectColor('yellow')">
                <span v-if="selectedColor === 'yellow'">
                    <font-awesome-icon :icon="['fas', 'check']" size="2xl" class="text-neutral-500" />
                </span>
            </div>
            <div class="circle border-white-600 border-2 bg-white t400-black" @click="selectColor('white')">
                <span v-if="selectedColor === 'white'">
                    <font-awesome-icon :icon="['fas', 'check']" size="2xl" class="text-neutral-500" />
                </span>
            </div>
            <div class="circle border-pink-600 border-2 bg-pink-400 text-white" @click="selectColor('pink')">
                <span v-if="selectedColor === 'pink'">
                    <font-awesome-icon :icon="['fas', 'check']" size="2xl" />
                </span>
            </div>
        </div>
        <div class="flex flex-col gap-4">
            <div class="flex gap-2" v-for="(row, rowIndex) in numberOfRows">
                <div
                    class="circle border-2"
                    v-for="(circle, circleIndex) in 4"
                    @click="applyColor(rowIndex, circleIndex)"
                    :class="!rowsData[rowIndex][circleIndex] ? 'bg-neutral-100' : rowsData[rowIndex][circleIndex] === 'white' ? 'bg-white' : 'bg-' + rowsData[rowIndex][circleIndex] + '-400'"
                ></div>
                <div class="ml-2" v-if="step === rowIndex">
                    <button class="button rounded-full w-[88px] h-full" :disabled="!(rowsData[rowIndex].filter((row) => row === '').length === 0)" @click="verifyAnswer()">Verifier</button>
                </div>
                <div class="ml-2" v-else>
                    <div class="flex gap-2 items-center justify-center w-full h-full">
                        <div class="border-2 w-4 h-4 rounded-full" :class="rowsHistory[rowIndex][historyIndex] ? 'bg-orange-400' : 'bg-neutral-100'" v-for="(history, historyIndex) in 4"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
export default {
    data() {
        return {
            selectedColor: null as string | null,
            numberOfRows: 10,
            rowsData: [] as string[][],
            rowsHistory: [] as boolean[][],
            answer: ['blue', 'green', 'red', 'yellow'],
            step: 0,
        }
    },
    created() {
        for (let i = 0; i < this.numberOfRows; i++) {
            this.rowsData.push([])
            for (let j = 0; j < 4; j++) {
                this.rowsData[i].push('')
            }
        }

        for (let i = 0; i < this.numberOfRows; i++) {
            this.rowsHistory.push([])
            for (let j = 0; j < 4; j++) {
                this.rowsHistory[i].push(false)
            }
        }

        // TODO: Randomize answer
    },
    methods: {
        selectColor(color: string) {
            if (this.selectedColor === color) {
                this.selectedColor = null
                return
            }
            this.selectedColor = color
        },

        applyColor(row: number, column: number) {
            if (this.step !== row || !this.selectedColor) {
                return
            }
            this.rowsData[row][column] = this.selectedColor
        },
        verifyAnswer() {
            if (
                this.rowsData[this.step][0] === this.answer[0] &&
                this.rowsData[this.step][1] === this.answer[1] &&
                this.rowsData[this.step][2] === this.answer[2] &&
                this.rowsData[this.step][3] === this.answer[3]
            ) {
                alert('You win!')
                return
            }

            if (this.rowsData[this.step][0] === this.answer[0]) {
                this.rowsHistory[this.step][0] = true
            }
            if (this.rowsData[this.step][1] === this.answer[1]) {
                this.rowsHistory[this.step][1] = true
            }
            if (this.rowsData[this.step][2] === this.answer[2]) {
                this.rowsHistory[this.step][2] = true
            }
            if (this.rowsData[this.step][3] === this.answer[3]) {
                this.rowsHistory[this.step][3] = true
            }

            console.log(this.rowsHistory)

            this.step += 1
        },
    },
}
</script>

<style scoped>
.circle {
    width: 3rem;
    height: 3rem;
    border-radius: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: all 0.1s ease;
    &:hover {
        filter: brightness(1.1);
    }
    &:active {
        filter: brightness(0.9);
    }
}

.button {
    background-color: #4a5568;
    color: white;
    font-weight: 600;
    transition: all 0.1s ease;
    &:hover {
        background-color: #2d3748;
    }
    &:active {
        background-color: #1a202c;
    }
    &:disabled {
        background-color: #cbd5e0;
        color: #a0aec0;
    }
}
</style>

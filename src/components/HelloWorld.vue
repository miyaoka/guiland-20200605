<template>
  <div>
    <h1>#金曜GUI</h1>
    <h2>ありよりのなしレンジスライダーやりたい</h2>

    <section class="control">
      <label>
        <div class="range">
          <p><input v-model="left" type="text" /></p>
          <input
            v-model.number="value"
            type="range"
            class="value"
            min="-1"
            max="1"
            step="any"
          />
          <p><input v-model="right" type="text" /></p>
        </div>
        <div class="depth">
          深み
          <input v-model.number="depth" type="range" min="2" max="6" step="1" />
          {{ depth }}
        </div>
        <div class="result">「{{ joinedValue }}」</div>
      </label>
    </section>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      value: 0,
      left: 'あり',
      right: '無し',
      depth: 4,
    }
  },
  computed: {
    binary(): string {
      return Math.floor(
        Math.abs(this.value) * parseInt('1'.repeat(this.depth), 2)
      ).toString(2)
    },
    computedValueList(): string[] {
      if (this.binary === '0') {
        return [[this.left, this.right, ''].join('か')]
      }
      const isNegative = this.value < 0
      const map: Record<string, string> = isNegative
        ? {
            '0': this.right,
            '1': this.left,
          }
        : {
            '0': this.left,
            '1': this.right,
          }
      const itemList = this.binary.split('').map((item) => map[item])
      return itemList.length === 1 ? [itemList[0] + 'かも'] : itemList
    },
    joinedValue(): string {
      let contCount = 0

      return this.computedValueList.reduce((acc, item, i, ary) => {
        const prev = ary[i - 1]
        const isCont = prev === item
        if (isCont && contCount === 0) {
          contCount++
        } else {
          contCount = 0
        }

        return (
          acc +
          (!prev || contCount === 1 ? '' : 'の') +
          (i + 1 === ary.length ? item : item.slice(0, 2))
        )
      }, '')
    },
  },
})
</script>

<style scoped lang="scss">
.value {
  width: 300px;
}
.range {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
.result {
  font-size: 24px;
  border: 1px solid #999;
  padding: 20px;
  border-radius: 20px;
  margin: 20px;
}

.values {
  display: flex;
  flex-direction: column;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

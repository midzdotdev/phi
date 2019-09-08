<template>
  <section class="section">
    <div class="columns">
      <div class="column is-3">
        <b-field label="Base Size:">
          <b-numberinput
            v-model="base"
            min="5"
            max="100"
          />
        </b-field>
      </div>
      <div class="column is-6">
        <b-field label="Text:">
          <b-input v-model="text" />
        </b-field>
      </div>
    </div>

    <b-table
      :data="tableData"
    >
      <template slot-scope="props">
        <b-table-column field="size" label="Size">
          {{ props.row.size }}
        </b-table-column>
        <b-table-column field="text" label="Text">
          <p :style="props.row.style">{{ text }}</p>
        </b-table-column>
      </template>>
    </b-table>
  </section>
</template>

<script>
const PHI = 1.61803398875

export default {
  name: 'Fonts',

  data () {
    return {
      base: 16,
      decimals: true,
      text: 'The quick brown fox'
    }
  },

  computed: {
    tableData () {
      const orders = [ 2, 1.5, 1, 0.5, 0, -0.5 ]

      return orders.map(o => {
        const size = this.base * Math.pow(PHI, o)

        return {
          size: size.toFixed(0),
          style: {
            fontSize: size + 'px'
          }
        }
      })
    }
  }
}
</script>

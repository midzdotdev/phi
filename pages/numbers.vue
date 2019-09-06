<template>
  <section class="section">
    <div class="columns">
      <div class="column">
        <b-field label="Base Value:">
          <b-input v-model="base"/>
        </b-field>
      </div>
      <div class="column">
        <b-field label="Decimal Places:">
          <b-numberinput
            v-model="dp"
            min="0"
            max="15"
          />
        </b-field>
      </div>
      <div class="column">
        <label class="label">Suffix:</label>
        <b-field>
          <b-radio-button
            v-model="suffix"
            native-value=""
          ><em>none</em></b-radio-button>
          <b-radio-button
            v-for="s in suffixes"
            :key="s"
            v-model="suffix"
            :native-value="s"
          >{{ s }}</b-radio-button>
        </b-field>
      </div>
    </div>

    <h3 class="is-size-3">Values</h3>
    <b-table
      :data="tableData"
      :narrowed="smallTable"
    >
      <template slot-scope="props">
        <b-table-column field="index" label="Multiplier">
          <b-button 
            @click="modifyIndex(props.row.index)"
            :size="smallTable ? 'is-small' : undefined"
          >
            &phi; <sup>{{ props.row.index }}</sup>
          </b-button>
        </b-table-column>
        <b-table-column field="value" label="Value">
          <b-button
            @click="copy(props.row.value)"
            :size="smallTable ? 'is-small' : undefined"
          >
            {{ props.row.value }}
          </b-button>
        </b-table-column>
      </template>
    </b-table>
  </section>
</template>

<script>
import Card from '~/components/Card'

const PHI = 1.61803398875
const INDICES_KEY = 'phi-indices'
const DEFAULT_INDICES = [-2, -1, -0.5, 0, 0.5, 1, 1.5, 2, 3, 4]

export default {
  name: 'HomePage',

  components: {
    Card
  },

  data () {
    return {
      base: 100,
      dp: 3,
      suffix: '',
      suffixes: [ 'px', 'em', 'pt' ],
      smallTable: false,
      indices: localStorage.getItem(INDICES_KEY) ? 
        JSON.parse(localStorage.getItem(INDICES_KEY))
        : DEFAULT_INDICES
    }
  },

  computed: {
    tableData () {
      return this.indices.map(index => ({
        index,
        value: (this.base * Math.pow(PHI, index)).toFixed(this.dp) + this.suffix
      }))
    }
  },

  methods: {
    modifyIndex (indexValue) {
      const i = this.indices.findIndex(x => x === indexValue)

      this.$buefy.dialog.prompt({
        message: `Enter the desired value for the order:`,
        inputAttrs: {
            type: 'text',
            placeholder: '',
            value: String(indexValue),
            min: -10,
            max: 10
        },
        onConfirm: value => {
          if (isNaN(value)) {
            this.$buefy.toast.open({
                duration: 2000,
                message: `It needs to be a number.`,
                type: 'is-danger'
            })
          }

          const n = parseFloat(value)
          this.indices.splice(i, 1, n)
          this.indices.sort((a, b) => a - b)

          this.saveIndices()
        }
      })
    },

    copy (str) {
      this.$copyText(str)
      this.$buefy.toast.open({
        duration: 2000,
        message: 'The value has been copied to the clipboard.',
        type: 'is-success'
      })
    },

    saveIndices () {
      const value = JSON.stringify(this.indices)
      localStorage.setItem(INDICES_KEY, value)
    }
  }
}
</script>

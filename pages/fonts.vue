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
    </div>

    <div class="container">
      <p
        v-for="(l, i) in lines"
        :key="i"
        :style="l.style"
      >{{ l.text }}</p>
    </div>
  </section>
</template>

<script>
const PHI = 1.61803398875

export default {
  name: 'Fonts',

  data () {
    return {
      base: 16
    }
  },

  computed: {
    lines () {
      const orders = [ 2, 1.5, 1, 0.5, 0, -0.5 ]

      return orders.map(o => {
        const size = this.base * Math.pow(PHI, o)

        return {
          text: size.toFixed(0),
          style: {
            fontSize: size + 'px'
          }
        }
      })
    }
  }
}
</script>

<template>
  <div>
    <nav
      class="navbar header has-shadow is-primary"
      role="navigation"
      aria-label="main navigation"
    >
      <div class="navbar-brand">
        <a
          class="navbar-item is-size-5 has-text-weight-bold"
          href="/"
        >Phi &nbsp; <span class="tag is-warning">beta</span></a>
      </div>

      <div class="navbar-menu">
        <div class="navbar-end">
          <div class="navbar-item">
            <b-field>
              <b-button tag="a"
                  href="https://github.com/james2mid/phi"
                  target="_blank"
                  class="is-dark"
                >
                  <b-icon icon="github-circle"/>
                  <span>Repo</span>
              </b-button>
            </b-field>
          </div>
          
          <a href="https://jamesmiddleton.me" class="navbar-item">
            By James Middleton
          </a>
        </div>
      </div>
    </nav>

    <section class="main-content columns">
      <aside class="column is-2 section">
        <ul class="menu-list">
          <li
            v-for="(item, key) of items"
            :key="key"
          >
            <nuxt-link
              :to="item.to"
              exact-active-class="is-active"
            >
              <b-icon :icon="item.icon" /> {{ item.title }}
            </nuxt-link>
          </li>
        </ul>
      </aside>

      <div class="container column is-10">
        <nuxt />
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data () {
    return {
      items: [
        {
          title: 'About',
          icon: 'information-outline',
          to: { name: 'index' }
        },
        {
          title: 'Numbers',
          icon: 'numeric',
          to: { name: 'numbers' }
        },
        {
          title: 'Fonts',
          icon: 'format-font',
          to: { name: 'fonts' }
        }
      ]
    }
  }
}
</script>

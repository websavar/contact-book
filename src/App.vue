<template>
  <div class="container">
    <Header
      :title="projectTitle"
      :show-add-contact="showAddContact"
      @search="searchContact"
      @toggle-add-contact="toggleAddContact"
    />
    <router-view
      :showAddContact="showAddContact"
      :search="search"
    ></router-view>
    <Footer />
  </div>
</template>

<script>
import './App.css'
import Header from './components/Headers'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Header,
    Footer
  },
  data () {
    return {
      projectTitle: 'Contact Book',
      showAddContact: false,
      search: ''
    }
  },
  methods: {
    toggleAddContact () {
      this.showAddContact = !this.showAddContact
    },
    searchContact (val) {
      this.search = val
    }
  }
}
</script>

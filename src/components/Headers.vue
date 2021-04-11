<template>
  <header class="pt-5">
    <div class="d-flex row align-items-center mb-2">
      <div class="d-flex col-sm-8 justify-content-sm-start justify-content-center">
        <h1 class="text-dark">{{ title }}</h1>
      </div>
      <div class="form-group d-flex col-sm-4 justify-content-sm-end justify-content-center m-0">
        <Button class="pb-2"
          v-show="homePage"
          @btn-click="$emit('toggle-add-contact')"
          :text="showAddContact ? 'close' : 'Add contact'"
          :color="showAddContact ? 'btn-outline-danger' : 'btn-info'" />
      </div>
      <div v-show="homePage" class="form-group search col-xs-12 col-sm-6 col-lg-4">
        <input
          @input="$emit('search', search)"
          v-model="search"
          type="search"
          class="form-control"
          name="search"
          placeholder="Search in contacts..." />
      </div>
    </div>
  </header>
</template>

<script>
import Button from './Button'

export default {
  name: 'Headers',
  props: {
    title: String,
    subtitle: String,
    showAddContact: Boolean
  },
  data () {
    return {
      search: ''
    }
  },
  components: {
    Button
  },
  computed: {
    homePage () {
      return (this.$route.path === '/')
    }
  }
}
</script>

<style scoped>
  .search {
    margin: 10px 5px;
  }
</style>

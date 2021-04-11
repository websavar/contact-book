<template>
  <div class="contacts">
    <div
      v-for="contact in contacts"
      :key="contact.id"
      class="contact"
    >
      <Contact
        :contact="contact"
        @toggle-favorite="$emit('toggle-favorite', contact.id)"
        @delete-contact="$emit('delete-contact', contact.id)"
      />
    </div>
  </div>
</template>

<script>
import Contact from './Contact'

export default {
  name: 'Contacts',
  components: {
    Contact
  },
  props: {
    contacts: Array
  }
}
</script>

<style>
  .contact:nth-child(2n) .contact-row {
    background: #fcf7ff;
  }
</style>

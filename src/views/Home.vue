<template>
  <main @search="search">
    <AddContact
      v-show="showAddContact"
      :show-add-contact="showAddContact"
      @add-contact="addContact"
    />
    <Contacts
      :contacts="contacts"
      @edited-contact="editedContact"
      @toggle-favorite="toggleFavorite"
      @delete-contact="deleteContact"
    />
    <div
      v-if="contacts == null"
      class="text-center mt-4"
    >
      Nothing found!
    </div>
  </main>
</template>

<script>
import Contacts from '../components/Contacts'
import AddContact from '../components/AddContact'

export default {
  name: 'Home',
  components: {
    Contacts,
    AddContact
  },
  props: {
    showAddContact: Boolean,
    search: String
  },
  data () {
    return {
      contacts: [],
      contactsList: []
    }
  },
  async created () {
    this.contacts = await this.fetchContacts()
    this.contactsList = this.contacts
  },
  beforeUpdate () {
    this.contacts = this.contactsList.filter(contact => {
      return contact.firstname.toLowerCase().includes(this.search.toLowerCase()) ||
        contact.lastname.toLowerCase().includes(this.search.toLowerCase()) ||
        contact.phone.includes(this.search)
    })
    if (this.contacts.length === 0) this.contacts = null
  },
  methods: {
    async addContact (contact) {
      const res = await fetch('api/phonebook', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(contact)
      })

      const data = await res.json()

      this.contacts = [data, ...this.contacts]
      this.contactsList = this.contacts
    },
    editedContact (updContact) {
      this.contacts = this.contacts.map(contact => (contact.id === updContact.id) ? (contact = updContact) : contact)
      this.contactsList = this.contacts
    },
    async deleteContact (id) {
      if (confirm('Are you sure you want to delete this contact?')) {
        const res = await fetch(`api/phonebook/${id}`, {
          method: 'DELETE'
        })
        res.status === 200 ? this.contacts = this.contacts.filter(contact => contact.id !== id) : alert('Error deleting contact')
      }
      this.contactsList = this.contacts
    },
    async toggleFavorite (id) {
      const contact = await this.fetchContact(id)
      const updatedContact = { ...contact, favorite: !contact.favorite }

      const res = await fetch(`api/phonebook/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'Application/json'
        },
        body: JSON.stringify(updatedContact)
      })

      const data = await res.json()

      this.contacts = this.contacts.map(contact => contact.id === id ? { ...contact, favorite: data.favorite } : contact)
      this.contactsList = this.contacts
    },
    async fetchContacts () {
      const res = await fetch('api/phonebook')
      const data = await res.json()

      data.sort(function (a, b) {
        var nameA = a.firstname.toUpperCase()
        var nameB = b.firstname.toUpperCase()
        if (nameA < nameB) return -1
        if (nameA > nameB) return 1
        return 0
      })

      return data
    },
    async fetchContact (id) {
      const res = await fetch(`api/phonebook/${id}`)
      const data = await res.json()
      return data
    }
  }
}
</script>

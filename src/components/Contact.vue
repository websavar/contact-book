<template>
  <div class="contact-row">
    <div ref="fname" class="single-line" @focus="select" @keyup.enter="saveContact">{{ contact.firstname }}</div>
    <div ref="lname" class="single-line" @focus="select" @keyup.enter="saveContact">{{ contact.lastname }}</div>
    <div ref="phone" class="single-line" @focus="select" @keyup.enter="saveContact">{{ contact.phone }}</div>
    <div class="icons">
      <i :class="[contact.favorite ? 'favorite' : '', 'icon-star star']" @click="$emit('toggle-favorite')" />
      <i ref="edit" class="icon-edit edit" @click="editContact" />
      <i ref="save" class="icon-save save" @click="saveContact" />
      <i class="icon-trash delete" @click="$emit('delete-contact', contact.id)" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Contact',
  props: {
    contact: Object
  },
  mounted () {
    this.numberformat()
  },
  methods: {
    select (e) {
      var range = document.createRange()
      range.selectNodeContents(e.target)
      var sel = window.getSelection()
      sel.removeAllRanges()
      sel.addRange(range)
    },
    editContact () {
      this.$refs.fname.contentEditable = true
      this.$refs.lname.contentEditable = true
      this.$refs.phone.contentEditable = true
      this.$refs.edit.style.display = 'none'
      this.$refs.save.style.display = 'block'
      this.$refs.fname.focus()
    },
    async saveContact () {
      this.$refs.fname.contentEditable = false
      this.$refs.lname.contentEditable = false
      this.$refs.phone.contentEditable = false
      this.$refs.edit.style.display = 'block'
      this.$refs.save.style.display = 'none'

      const editedContact = {
        firstname: this.removeHTML(this.$refs.fname.innerHTML).trim(),
        lastname: this.removeHTML(this.$refs.lname.innerHTML).trim(),
        phone: this.$refs.phone.innerHTML.replace(/\D+/g, '').trim(),
        favorite: false,
        id: this.contact.id
      }

      this.$emit('edited-contact', editedContact)

      await fetch(`api/phonebook/${this.contact.id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'Application/json'
        },
        body: JSON.stringify(editedContact)
      })

      this.numberformat()
    },
    removeTags (string) {
      return string.replace(/(<([^>]+)>)/ig, ' ')
    },
    removeHTML (str) {
      var tmp = document.createElement('DIV')
      tmp.innerHTML = str
      return tmp.textContent || tmp.innerText || ''
    },
    numberformat () {
      var x = this.$refs.phone.innerHTML.replace(/\D/g, '').match(/(\d{0,3})(\d{0,3})(\d{0,4})/)
      this.$refs.phone.innerHTML = !x[2] ? x[1] : '(' + x[1] + ') ' + x[2] + (x[3] ? '-' + x[3] : '')
    }
  }
}
</script>

<style scope>
.contact-row {
  align-items: center;
  background: #f0f0f0;
  column-gap: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 0.5fr;
  margin: 5px;
  padding: 10px 20px;
}
.icons {
  display: flex;
  justify-content: space-around;
}
.icons i {
  cursor: pointer;
}
.edit, .delete {
  opacity: 0;
  transition: 0.05s ease-in-out;
  visibility: hidden;
}
.star {
  color: #ccc;
}
.edit, .save {
  color: green;
}
.save {
  display: none;
}
.delete {
  color: red;
}
.favorite.star {
  color: #ffc41f;
}
.contact-row:hover .delete, .contact-row:hover .edit {
  opacity: 1;
  visibility: visible;
}
div[contenteditable="true"] {
  background: white;
  border: 1px solid #b2b2b2;
  border-radius: 3px;
  padding: 0px 5px;
  outline: none;
}
.single-line {
  white-space: nowrap;
  overflow: hidden;
}
.single-line br {
  display: none;
}
.single-line * {
  display:inline;
  white-space:nowrap;
}
@media only screen and (max-width: 500px) {
  .contact-row {
    column-gap: 5px;
    display: grid;
    grid-template-columns: 0.8fr 0.8fr 1fr 0.5fr;
    margin: 5px 0;
    padding: 5px;
  }
  .edit, .delete {
    opacity: 1;
    visibility: visible;
  }
  .single-line {
    background: #dedce1;
  }
}
</style>

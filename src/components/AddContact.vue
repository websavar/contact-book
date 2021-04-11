<template>
  <form class="add-form" @submit="onSubmit">
    <div class="row col-md-12">
      <div class="form-group col-12 col-sm-6 col-md-3">
        <input
          v-model="firstname"
          type="text"
          class="form-control"
          name="firstname"
          placeholder="Name"
          required>
      </div>
      <div class="form-group col-12 col-sm-6 col-md-3">
        <input
          v-model="lastname"
          type="text"
          class="form-control"
          name="lastname"
          placeholder="Last name">
      </div>
      <div class="form-group col-12 col-sm-6 col-md-3">
        <input
          v-model="phone"
          type="text"
          class="form-control"
          name="phone"
          placeholder="Phone number"
          required
          @input="numberformat">
      </div>
      <div class="form-group d-flex align-items-center col-2 col-sm-1 col-lg-1 mb-1">
        <input
          id="favorite"
          v-model="favorite"
          name="favorite"
          type="checkbox"
        >
        <label for="favorite" title="Add to favorite contacts"/>
      </div>
      <div class="form-group submit d-flex justify-content-end col-10 col-sm-5 col-md-2">
        <input type="submit" value="Save Contact" class="btn btn-info">
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: 'AddContact',
  data () {
    return {
      firstname: '',
      lastname: '',
      phone: '',
      favorite: false
    }
  },
  methods: {
    onSubmit (e) {
      e.preventDefault()

      const newContact = {
        firstname: this.firstname.trim(),
        lastname: this.lastname.trim(),
        phone: this.phone.replace(/\D+/g, '').trim(),
        favorite: this.favorite
      }

      this.$emit('add-contact', newContact)

      this.firstname = ''
      this.lastname = ''
      this.phone = ''
      this.favorite = false
    },
    numberformat () {
      var x = this.phone.replace(/\D/g, '').match(/(\d{0,3})(\d{0,3})(\d{0,4})/)
      this.phone = !x[2] ? x[1] : '(' + x[1] + ') ' + x[2] + (x[3] ? '-' + x[3] : '')
    }
  }
}
</script>

<style scoped>
.row {
  box-sizing: content-box;
  margin-bottom: 10px;
}
.col-12, .col-12, .submit {
  padding: 5px !important;
  margin-bottom: 5px;
}
input[type=checkbox] {
  display:none;
}
input[type=checkbox] + label:before {
  cursor: pointer;
  display: inline-block;
  font-family: FontAwesome;
  margin: 0;
}
input[type=checkbox] + label:before {
  color: #ccc;
  content: "\f006";
}
input[type=checkbox]:checked + label:before {
  color: #ffc41f;
  content: "\f005";
}
label {
  margin: 0;
}
</style>

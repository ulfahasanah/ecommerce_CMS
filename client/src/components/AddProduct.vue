<template>
  <div>
    <div class="col-md-4 float-right mb-2" v-if="role === 'admin'">
          <b-button v-b-modal.modal-prevent-closing variant="info">Add New Product</b-button>
    </div>
      <b-modal id="modal-prevent-closing" ref="modal" title="Add New Product" @ok="handleOk">
        <Error></Error>
        <b-form ref="form" @submit.prevent="handleSubmit">
          <b-form-group id="input-group-1" label="Product Name:" label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="form.name"
              type="text"
              required
              placeholder="Enter name product"
            ></b-form-input>
          </b-form-group>
          <b-form-group id="input-group-2" label="Url Image:" label-for="input-2">
            <b-form-input
              id="input-2"
              v-model="form.image_url"
              required
              placeholder="Enter url image"
            ></b-form-input>
          </b-form-group>

          <b-form-group id="input-group-3" label="Price:" label-for="input-3">
            <b-form-input id="input-3" v-model="form.price" required placeholder="Enter price" type="number"></b-form-input>
          </b-form-group>

          <b-form-group id="input-group-4" label="Stock:" label-for="input-4">
            <b-form-input id="input-4" v-model="form.stock" required placeholder="Enter stock" type="number"></b-form-input>
          </b-form-group>
        </b-form>
      </b-modal>
  </div>
</template>

<script>
import { mapActions, mapMutations } from 'vuex'
import Error from './Error'

export default {
  name: 'AddProduct',
  components: {
    Error
  },
  data () {
    return {
      form: {
        name: '',
        image_url: '',
        price: '',
        stock: '',
        role: ''
      },
      modalShow: false
    }
  },
  methods: {
    ...mapActions(['addProduct', 'fetchProduct']),
    ...mapMutations(['SET_LOADING', 'SHOW_ERROR']),
    handleOk (bvModalEvt) {
      bvModalEvt.preventDefault()
      this.handleSubmit()
    },
    handleSubmit () {
      const payload = this.form
      this.addProduct(payload)
        .then(() => {
          this.fetchProduct()
          this.SHOW_ERROR('')
          this.$nextTick(() => {
            this.$bvModal.hide('modal-prevent-closing')
          })
        })
        .catch(err => {
          this.SHOW_ERROR(err.response.data.error)
        })
    }
  },
  created () {
    this.SHOW_ERROR('')
    this.role = localStorage.getItem('role')
  }
}

</script>

<style>

</style>

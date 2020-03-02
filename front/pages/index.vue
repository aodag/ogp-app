<template>
  <div class="container">
    <header>
      <h1 class="title">OGP.app</h1>
    </header>
    <div class="content">
      <div class="form">
        <a-form :form="form" @submit="handleSubmit">
          <a-form-item
            :validate-status="wordsError() ? 'error' : ''"
            :help="wordsError() || ''"
          >
            <a-input
              v-decorator="[
                'words',
                {
                  rules: [
                    { required: true, message: 'Please input your words!' }
                  ]
                }
              ]"
              placeholder="What's happening?"
            >
              <!-- <a-icon slot="prefix" type="user" style="color:rgba(0,0,0,.25)" /> -->
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-button
              type="primary"
              html-type="submit"
              :disabled="hasErrors(form.getFieldsError())"
            >
              OGP !!!
            </a-button>
          </a-form-item>
        </a-form>
      </div>
    </div>
    <site-footer />
  </div>
</template>

<script>
import SiteFooter from '~/components/Footer.vue'

function hasErrors(fieldsError) {
  return Object.keys(fieldsError).some((field) => fieldsError[field])
}
export default {
  components: {
    SiteFooter
  },
  data() {
    return {
      hasErrors,
      form: this.$form.createForm(this, { name: 'horizontal_login' })
    }
  },
  mounted() {
    this.$nextTick(() => {
      // To disabled submit button at the beginning.
      this.form.validateFields()
    })
  },
  methods: {
    // Only show error after a field is touched.
    wordsError() {
      const { getFieldError, isFieldTouched } = this.form
      return isFieldTouched('words') && getFieldError('words')
    },
    handleSubmit(e) {
      e.preventDefault()
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values)
        }
      })
    }
  }
}
</script>

<style>
.container {
  margin: 40px auto 0;
  display: flex;
  justify-content: center;
  align-items: top;
  flex-direction: column;
  text-align: center;
}

.title {
  font-family: 'Lato', sans-serif;
  font-weight: 900;
  font-size: 48px;
  color: #35495e;
  letter-spacing: 0.5px;
}

.form {
  margin: 0 auto;
  max-width: 320px;
}

.form input {
  height: 40px;
  font-family: sans-serif;
  font-size: 16px;
}

.ant-form-explain {
  margin: 10px 0 8px;
}

.links {
  padding-top: 15px;
}
</style>

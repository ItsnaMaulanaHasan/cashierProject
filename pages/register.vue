<template>
  <v-row>
    <v-col cols="10" offset="1" md="4" offset-md="4">
      <v-card class="mb-2">
        <v-toolbar color="primary" dark>Register</v-toolbar>
        <v-card-text>
          <v-form>
            <v-text-field
              name="fullname"
              label="Full Name"
              type="text"
              :rules="rules.fullname"
              v-model="form.fullname"
            />
            <v-text-field
              name="email"
              label="Email"
              type="email"
              :rules="rules.email"
              v-model="form.email"
              @keyup="checkEmail"
            />
            <v-text-field
              name="password"
              label="Password"
              type="password"
              :rules="rules.password"
              v-model="form.password"
            />
            <v-text-field
              name="retype_password"
              label="Re-password"
              type="password"
              :rules="rules.retype_password"
              v-model="form.retype_password"
            />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn @click="onSubmit" color="primary" :disabled="isDisable">
            <span v-if="!isDisable">Register</span>
            <v-progress-circular v-else color="primary" indeterminate>
            </v-progress-circular>
          </v-btn>
        </v-card-actions>
      </v-card>
      <p>Kamu sudah punya akun? <v-btn to="/login" plain>login</v-btn></p>
    </v-col>
  </v-row>
</template>

<script>
export default {
  middleware: ['unauthenticated'],
  data() {
    return {
      emailExist: false,
      isDisable: false,
      form: {
        fullname: '',
        email: '',
        password: '',
        retype_password: '',
      },
      rules: {
        fullname: [
          (v) => !!v || this.$t('FIELD_REQUIRED', { field: 'Fullname' }),
        ],
        email: [
          (v) => !!v || this.$t('FIELD_REQUIRED', { field: 'Email' }),
          (v) => /.+@.+/.test(v) || this.$t('EMAIL_INVALID'),
          // (v) => !!this.emailExist || 'Email already exist',
        ],
        password: [
          (v) => !!v || this.$t('FIELD_REQUIRED', { field: 'Password' }),
          (v) =>
            v.length >= 6 ||
            this.$t('FIELD_MIN', { field: 'Password', min: 6 }),
        ],
        retype_password: [
          (v) =>
            v === this.form.password ||
            this.$t('FIELD_CONFIRM', {
              field: 'Password',
              confirm: 'Re-Password',
            }),
        ],
      },
    }
  },
  methods: {
    checkEmail() {
      this.$axios
        .$post('http://localhost:3000/auth/check-email', this.form)
        .then((response) => {
          this.emailExist = false
        })
        .catch((error) => {
          this.emailExist = true
        })
    },
    onSubmit() {
      this.isDisable = true
      this.$axios
        .$post('http://localhost:3000/auth/register', this.form)
        .then((response) => {
          this.isDisable = false
          //REDIRECT TO LOGIN PAGE
          this.$router.push('/login')
        })
        .catch((error) => {
          this.isDisable = false
        })
    },
  },
}
</script>

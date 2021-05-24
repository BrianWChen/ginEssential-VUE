<template>
  <div class="register">
    <b-row class="mt-5">
      <b-col
        md="8"
        offset-md="2"
        lg="6"
        offset-lg="3"
      >
        <b-card title="register">
          <b-form>
            <b-form-group label="Username">
              <b-form-input
                v-model="$v.user.name.$model"
                type="text"
                placeholder="Enter Username"
              ></b-form-input>
            </b-form-group>
            <b-form-group label="Telephone">
              <b-form-input
                v-model="$v.user.telephone.$model"
                type="number"
                placeholder="Enter Telephone"
              ></b-form-input>
              <b-form-invalid-feedback :state="validateState('telephone')">
                Telephone == 11.
              </b-form-invalid-feedback>
              <b-form-valid-feedback :state="validateState('telephone')">
                Looks Good.
              </b-form-valid-feedback>
            </b-form-group>
            <b-form-group description="We'll never share your email with anyone else.">
              <b-form-input
                v-model="$v.user.password.$model"
                type="password"
                placeholder="Enter Password"
              ></b-form-input>
              <b-form-invalid-feedback :state="validateState('password')">
                Password >= 6.
              </b-form-invalid-feedback>
              <b-form-valid-feedback :state="validateState('password')">
                Looks Good.
              </b-form-valid-feedback>
            </b-form-group>
            <b-form-group>
              <b-button
                @click="register"
                block
                variant="outline-primary"
              >Submit</b-button>
            </b-form-group>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>
<script>
import { required, minLength } from 'vuelidate/lib/validators';

import customValidator from '@/helper/validator';
import { mapActions } from 'vuex';

export default {
  data() {
    return {
      user: {
        name: '',
        telephone: '',
        password: '',
      },
      validation: null,
    };
  },
  validations: {
    user: {
      name: {

      },
      telephone: {
        required,
        telephone: customValidator.telephoneValidator,
      },
      password: {
        required,
        minLength: minLength(6),
      },
    },
  },
  methods: {
    ...mapActions('userModule', { userRegister: 'register' }),
    validateState(name) {
      const { $dirty, $error } = this.$v.user[name];
      return $dirty ? !$error : null;
    },
    register() {
      this.$v.user.$touch();
      if (this.$v.user.$anyError) {
        return;
      }
      this.userRegister(this.user).then(() => {
        this.$router.replace({ name: 'Home' });
      }).catch((err) => {
        this.$bvToast.toast(err.response.data.msg, {
          title: 'wrong data',
          variant: 'danger',
          solid: true,
        });
      });
      console.log('register');
    },
  },
};
</script>
<style lang="scss" scoped>
</style>

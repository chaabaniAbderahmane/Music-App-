<template>
  <div>
    <div
      class="text-white text-center font-bold p-4 rounded mb-4"
      v-if="reg_show_alert"
      :class="reg_alert_variant"
    >
      {{ reg_alert_msg }}
    </div>
    <!-- :validation-schema="schema" -->
    <vee-form @submit="register" :validation-schema="schema" :initial-values="userdata">
      <!-- Name -->
      <div class="mb-3">
        <label class="inline-block mb-2">{{ $t('register.name') }}</label>
        <vee-field
          type="text"
          name="name"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          :placeholder="$t('ph.name')"
        />
        <ErrorMessage class="text-red-600" name="name" />
      </div>
      <!-- Email -->
      <div class="mb-6">
        <label class="inline-block mb-2">email</label>
        <vee-field
          name="email"
          type="email"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          :placeholder="$t('ph.email')"
        />
        <ErrorMessage class="text-red-600" name="email" />
      </div>
      <!-- Gender -->

      <div class="mb-4">
        <label class="block mb-2">{{ $t('register.gender') }}</label>
        <vee-field name="gender">
          <label class="inline-flex items-center">
            <vee-field
              name="gender"
              type="radio"
              value="Male"
              class="form-radio h-5 w-5 text-blue-600"
              v-model="genderr"
              @change="onGenderChange('Male')"
            />
            <span class="ml-2">{{ $t('register.male') }}</span>
          </label>
          <label class="inline-flex items-center ml-6">
            <vee-field
              name="gender"
              type="radio"
              value="Female"
              class="form-radio h-5 w-5 text-red-600"
              v-model="genderr"
              @change="onGenderChange('Female')"
            />
            <span class="ml-2">{{ $t('register.female') }}</span>
          </label>
        </vee-field>
        <br />
        <ErrorMessage class="text-red-600" name="gender" />
      </div>

      <!-- Age -->
      <div class="mb-3">
        <label class="inline-block mb-2">{{ $t('register.age') }}</label>
        <vee-field
          name="age"
          type="number"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          :placeholder="$t('register.age')"
        />
        <ErrorMessage class="text-red-600" name="age" />
      </div>
      <!-- Password -->
      <div class="mb-3">
        <label class="inline-block mb-2">{{$t('Login.password')}}</label>

        <vee-field name="password" v-slot="{ field, errors }" :bails="false">
          <input
            name="iput"
            type="password"
            class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
            :placeholder="$t('Login.password')"
            v-bind="field"
          />

          <div class="text-red-600" v-for="error in errors" :key="error">{{ error }}</div>
        </vee-field>
      </div>
      <!-- Confirm Password -->
      <div class="mb-3">
        <label class="inline-block mb-2">{{ $t('register.confirm') }}</label>
        <vee-field
          name="confirm_password"
          type="password"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          :placeholder="$t('register.confirm')"
        />
        <ErrorMessage class="text-red-600" name="confirm_password" />
      </div>
      <!-- Country -->
      <div class="mb-3">
        <label class="inline-block mb-2">{{ $t('register.country') }}</label>
        <vee-field
          as="select"
          name="country"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
        >
          <option value="USA">{{ $t('register.usa') }}</option>
          <option value="Mexico">{{ $t('register.mexico') }}</option>
          <option value="Germany">{{ $t('register.germany') }}</option>
          <option value="Antartica">{{ $t('register.antartica') }}</option>
        </vee-field>
        <ErrorMessage class="text-red-600" name="country" />
      </div>
      <!-- TOS -->
      <div class="mb-3 pl-6">
        <vee-field
          type="checkbox"
          name="tos"
          class="w-4 h-4 float-left -ml-6 mt-1 rounded"
          value="1"
        />
        <i18n-t class="inline-block" keypath="register.accept" tag="label">
          <a href="#"> {{ $t('register.tos') }} </a>
        </i18n-t>
      </div>
      <ErrorMessage class="text-red-600" name="tos" />

      <button
        type="submit"
        class="block w-full bg-purple-600 text-white py-1.5 px-3 rounded transition hover:bg-purple-700"
        :disabled="reg_in_submission"
      >
        {{ $t('CompositionItem.Submit') }}
      </button>
    </vee-form>
  </div>
</template>

<script>
import { mapActions } from 'pinia'
import useUserStore from '@/stores/user'

export default {
  name: 'AppRegister',
  props: {
    Vis: Function
  },

  data() {
    return {
      reg_in_submission: false,
      reg_alert_msg: 'Please wait , your account is been created .',
      reg_alert_variant: 'bg-blue-500',
      reg_show_alert: false,
      genderr: '',
      itschecked: false,
      schema: {
        name: 'required|min:3|max:10|AlphaSpaces',
        email: 'required|email',
        gender: 'required',
        age: 'required|min_value:18|max_value:60',
        password: 'required|min:3|max:10|is_not:password',
        confirm_password: 'required|pass_mismatch:@password',
        country: 'required|country_excluded:Antartica,Antarctique,Antarctica',
        tos: 'tos'
      },

      userdata: {
        country: 'USA'
      }
    }
  },

  methods: {
    ...mapActions(useUserStore, {
      createuser: 'register'
    }),

    onGenderChange(selectedGender) {
      this.gender = selectedGender
      this.itschecked = true
    },
    async register(values) {
      this.reg_show_alert = true
      this.reg_in_submission = true
      this.reg_alert_msg = 'Please wait , your account is been created .'
      this.reg_alert_variant = 'bg-blue-500'
      if (!this.itschecked) {
        this.gender = ''
      }

      try {
        this.createuser(values, this.gender)
        console.log(values)
      } catch (error) {
        console.error('User creation error:', error)
        this.reg_alert_variant = 'bg-red-500'
        this.reg_alert_msg = 'Something went wrong , try later again '
        this.reg_in_submission = false

        return
      }

      this.reg_alert_msg = 'Your account has been created'
      this.reg_alert_variant = 'bg-green-700'
    }
  }
}
</script>

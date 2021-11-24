<template>
  <form @submit.prevent="onSubmit">
    <!-- Lazy Validation: we want to listen to change and not imput -->
    <BaseInput
      label="Email"
      type="email"
      :error="emailError"
      :modelValue="email"
      @change="handleChange"
    />

    <BaseInput
      label="Password"
      type="password"
      v-model="password"
      :error="passwordError"
    />

    <BaseButton type="submit" class="-fill-gradient"> Submit </BaseButton>
  </form>
</template>

<script>
import { useField, useForm } from 'vee-validate';

export default {
  setup() {
    function onSubmit() {
      alert('Submitted');
    }

    const validations = {
      // WARNING : this naming 'email' & 'password' must match the name of the 'model' passed as first argument inside 'useField' methods l.50 & 52
      email: (value) => {
        if (!value) return 'This field is required';

        const regex =
          /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
        if (!regex.test(String(value).toLowerCase())) {
          return 'Please enter a valid email address';
        }
        return true;
      },
      password: (value) => {
        const requiredMessage = 'This field is required';
        if (value === undefined || value === null) return requiredMessage;
        if (!String(value).length) return requiredMessage;

        return true;
      },
    };

    // setFieldValue function will trigger the validation rules on the form’s field who was modified,
    // which would not be the case if we modified it directly.
    // see more : https://vee-validate.logaretm.com/v4/api/use-form#composable-api
    const { setFieldValue } = useForm({
      validationSchema: validations,
    });

    const { value: email, errorMessage: emailError } = useField('email');
    const { value: password, errorMessage: passwordError } =
      useField('password');

    const handleChange = (event) => {
      setFieldValue('email', event.target.value);
    };

    return {
      onSubmit,
      email,
      emailError,
      password,
      passwordError,
      handleChange,
    };
  },
};
</script>

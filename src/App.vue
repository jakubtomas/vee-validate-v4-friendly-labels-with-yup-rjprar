<template>
  <h3>Form VeeValidate</h3>
  <Form
    @submit="onSubmit"
    :validation-schema="schema"
    v-slot="{ values, errors, isSubmitting }"
  >
    <!-- v-slot="{ errors, isSubmitting, handleSubmit, resetForm, setErrors, setValues } -->
    {{ errors }}
    {{ errors.email_addr }}
    values
    <pre>{{ values }}</pre>
    <br />
    {{ isSubmitting }}
    <!-- up code is another way how to display error message 
    v slot have function and data  -->
    ------------------------------------------

    <label for="email">Your email</label>
    <Field
      name="email_addr"
      value=""
      type="email"
      v-slot="{ field, errorMessage, meta }"
    >
      <input v-bind="field" />
      Field
      <span>{{ field }}</span>
      <br />
      ErrorMessage
      <br />
      <span>{{ errorMessage }}</span>
      <br />
      Meta
      <pre>{{ meta }}</pre>
      <br />
      ---------------------------------------------------
      <br />
      <span v-if="errorMessage && !meta.valid">⛔️ {{ errorMessage }}</span>
      <span v-if="meta.valid && meta.touched">✅ Field is valid</span>
      <br />
      <ErrorMessage name="email_addr" />
      <br />
      --------------------------------------------------
    </Field>

    <!-- <label for="email">Your Email</label>
    <Field id="email_addr" name="email_addr" type="email" />
    <ErrorMessage name="email_addr" /> -->
    <br />
    -------------------------------------
    <!-- Every Field  has 3 data , field, erroMessage, meta and with thoses you can work with errorsMesssage -->
    <!-- Maybe also Form has data like  v-slot="{ errors, isSubmitting }" -->

    <label for="password">Your Password</label>
    <Field id="password" name="acc_pazzword" type="password" />
    <ErrorMessage name="acc_pazzword" />

    <!--  is possible wo add rules to Field -->
    <!-- <Field name="email" type="email" :rules="validateEmail" /> -->

    <button>Submit</button>
  </Form>
</template>

<script setup lang="ts">
import { Field, Form, ErrorMessage } from 'vee-validate';
import * as Yup from 'yup';

const schema = Yup.object().shape({
  email_addr: Yup.string().email().required().label('Emaill Address'),
  acc_pazzword: Yup.string().min(5).required().label('Your Password'),
});

function onSubmit(values) {
  alert(JSON.stringify(values, null, 2));
}

// function for rules Field
function validateEmail(value) {
  // if the field is empty
  if (!value) {
    return 'This field is required';
  }
  // if the field is not a valid email
  const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
  if (!regex.test(value)) {
    return 'This field must be a valid email';
  }
  // All is good
  return true;
}
</script>

<style>
input,
label {
  display: block;
  margin-top: 20px;
}

input + span {
  display: block;
}

button {
  margin-top: 20px;
}
</style>

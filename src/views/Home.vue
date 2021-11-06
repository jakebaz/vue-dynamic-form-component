<template>
  <h1>Homepage</h1>
  <Form
    :formFields="formFields"
    :schema="rules"
    @input-change="handleChange"
    @set-error="handleSetError"
    @delete-error="handleDeleteError"
    @submit="handleSubmit"
  />
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import Form from "@/components/Form.vue";
import { FormField } from "@/interfaces/FormField.interface";
import {
  maxLength,
  required,
  minLength,
  email,
  helpers,
} from "@vuelidate/validators";

export default defineComponent({
  components: {
    Form,
  },
  setup() {
    const formFields = ref<{ [key: string]: FormField }>({
      firstName: {
        name: "firstName",
        label: "First name",
        type: "text",
        value: "",
        error: "",
      },
      lastName: {
        name: "lastName",
        label: "Last name",
        type: "text",
        value: "",
        error: "",
      },
      email: {
        name: "email",
        label: "Email address",
        type: "email",
        value: "",
        error: "",
      },
      password: {
        name: "password",
        label: "Password",
        type: "password",
        value: "",
        error: "",
      },
      confirmPassword: {
        name: "confirmPassword",
        label: "Confirm Password",
        type: "password",
        value: "",
        error: "",
      },
    });

    const rules = {
      firstName: {
        value: {
          required,
        },
      },
      lastName: {
        value: {
          required,
        },
      },
      email: {
        value: {
          email: helpers.withMessage(
            "Please enter a valid email address",
            email
          ),
          required: helpers.withMessage(
            "Please enter a valid email address",
            required
          ),
        },
      },
      password: {
        value: {
          required,
          minLength: helpers.withMessage(
            "Password must be more than 8 characters",
            minLength(8)
          ),
          maxLength: helpers.withMessage(
            "Password must be less than 30 characters",
            maxLength(30)
          ),
        },
      },
      confirmPassword: {
        value: {
          required,
          minLength: helpers.withMessage(
            "Password must be more than 8 characters",
            minLength(8)
          ),
          maxLength: helpers.withMessage(
            "Password must be less than 30 characters",
            maxLength(30)
          ),
        },
      },
    };

    const handleChange = (data: { name: string; value: string }) =>
      (formFields.value[data.name].value = data.value);

    const handleSetError = (data: { name: string; message: string }) =>
      (formFields.value[data.name].error = data.message);

    const handleDeleteError = (name: string) =>
      (formFields.value[name].error = "");

    const handleSubmit = () => console.log("form submitted", formFields.value);

    return {
      formFields,
      rules,
      handleChange,
      handleSetError,
      handleDeleteError,
      handleSubmit,
    };
  },
});
</script>

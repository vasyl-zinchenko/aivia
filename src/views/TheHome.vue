<script setup>
import { reactive, ref } from "vue";
import useVuelidate from "@vuelidate/core";
import { required, email, minLength } from "@vuelidate/validators";
import { computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const visible = ref(false);

const formData = reactive({
  email: "",
  password: "",
});

const rules = computed(() => {
  return {
    email: { required, email },
    password: { required, minLength: minLength(6) },
  };
});

const v$ = useVuelidate(rules, formData);

const submitForm = async () => {
  await v$.value.$validate();
  
  if (!v$.value.$error) {
    router.push("/game");
  }
};
</script>

<template>
  <v-card
    class="mx-auto pa-12 pb-8 mt-10"
    elevation="8"
    max-width="448"
    rounded="lg"
  >
    <v-form fast-fail @submit.prevent>
      <div class="font-weight-light text-medium-emphasis">Email</div>

      <v-text-field
        class="pa-0"
        v-model="formData.email"
        density="compact"
        hide-details="auto"
        placeholder="Email address"
        prepend-inner-icon="mdi-email-outline"
        variant="outlined"
        :rules="v$.email.$errors"
      ></v-text-field>

      <span
        class="d-flex font-weight-light text-red mb-2"
        v-for="error in v$.email.$errors"
        :key="error.$uid"
        >{{ error.$message }}</span
      >

      <div
        class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between"
      >
        Password
      </div>

      <v-text-field
        v-model="formData.password"
        :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
        :type="visible ? 'text' : 'password'"
        density="compact"
        hide-details="auto"
        placeholder="Enter your password"
        prepend-inner-icon="mdi-lock-outline"
        variant="outlined"
        @click:append-inner="visible = !visible"
      ></v-text-field>

      <span
        class="d-flex font-weight-light text-red mb-2"
        v-for="error in v$.password.$errors"
        :key="error.$uid"
        >{{ error.$message }}</span
      >

      <v-btn
        block
        class="mb-8 mt-5"
        color="blue"
        size="large"
        variant="tonal"
        @click="submitForm"
      >
        Log In
      </v-btn>
    </v-form>
  </v-card>
</template>

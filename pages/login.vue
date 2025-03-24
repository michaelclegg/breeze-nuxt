<script setup lang="ts">
definePageMeta({ middleware: ["guest"] });

const router = useRouter();
const route = useRoute();
const { login } = useAuth();

const data = reactive({
  email: "",
  password: "",
  remember: false,
});
const status = ref(
  (route.query.reset ?? "").length > 0 ? atob(route.query.reset as string) : ""
);

const {
  submit,
  inProgress,
  validationErrors: errors,
} = useSubmit(
  () => {
    status.value = "";
    return login(data);
  },
  {
    onSuccess: () => router.push("/dashboard"),
  }
);
</script>

<template>
  <AuthCard>
    <template #logo>
      <NuxtLink to="/">
        <ApplicationLogo class="w-20 h-20 fill-current text-gray-500" />
      </NuxtLink>
    </template>

    <!-- Session Status -->
    <AuthSessionStatus class="mb-4" :status="status" />

    <form @submit.prevent="submit">
      <!-- Email Address -->
      <UFormField label="Email" for="email">
        <UInput
          id="email"
          type="email"
          class="block mt-1 w-full"
          v-model="data.email"
          :error="errors.email?.[0]"
          required
          autoFocus
        />
      </UFormField>

      <!-- Password -->
      <UFormField label="Password" for="password" class="mt-4">
        <UInput
          id="password"
          type="password"
          class="block mt-1 w-full"
          v-model="data.password"
          :error="errors.password"
          required
          autoComplete="current-password"
        />
      </UFormField>

      <!-- Remember Me -->
      <div class="block mt-4">
        <UCheckbox
          id="remember"
          v-model="data.remember"
          class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
        >
          <span class="ml-2 text-sm text-gray-600"> Remember me </span>
        </UCheckbox>
      </div>

      <div class="flex items-center justify-end mt-4">
        <NuxtLink
          href="/forgot-password"
          class="underline text-sm text-gray-600 hover:text-gray-900"
        >
          Forgot your password?
        </NuxtLink>

        <UButton class="ml-3" :disabled="inProgress">Login</UButton>
      </div>
    </form>
  </AuthCard>
</template>

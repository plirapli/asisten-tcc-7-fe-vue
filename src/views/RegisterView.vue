<script setup lang="ts">
import { defaultInstance } from "@/api/instance";
import { Alert, AlertTitle } from "@/components/ui/alert";
import { Button } from "@/components/ui/button";
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import router from "@/router";
import { AlertCircleIcon } from "lucide-vue-next";
import { reactive } from "vue";

const state = reactive({
  form: { name: "", email: "", gender: "Male", password: "" },
  msg: "",
  isLoading: false,
});

const registerHandler = async () => {
  try {
    state.msg = "";
    state.isLoading = true;
    const response = await defaultInstance.post("/login", state.form);
    localStorage.setItem("accessToken", response.data.accessToken);
    router.push("/");
  } catch (error: any) {
    state.msg =
      error.response?.data?.message || "Terjadi kesalahan saat login.";
  } finally {
    state.isLoading = false;
  }
};
</script>

<template>
  <div class="flex min-h-svh w-full items-center justify-center p-6 md:p-10">
    <div class="w-full max-w-sm">
      <Card class="mx-auto max-w-sm">
        <CardHeader>
          <Alert v-if="state.msg" variant="destructive" class="mb-2">
            <AlertCircleIcon />
            <AlertTitle>{{ state.msg }}</AlertTitle>
          </Alert>
          <CardTitle class="text-2xl">Create a new account</CardTitle>
          <CardDescription>
            Enter your data below to register a new account.
          </CardDescription>
        </CardHeader>
        <CardContent>
          <form @submit.prevent="registerHandler">
            <div class="grid gap-4">
              <div class="grid gap-2">
                <Label for="email">Email</Label>
                <Input
                  id="email"
                  v-model="state.form.email"
                  type="email"
                  placeholder="m@example.com"
                  required
                />
              </div>
              <div class="grid gap-2">
                <Label for="password">Password</Label>
                <Input
                  v-model="state.form.password"
                  id="password"
                  type="password"
                  required
                />
              </div>
              <Button type="submit" class="w-full cursor-pointer">
                Register
              </Button>
            </div>
          </form>
          <div class="mt-4 text-center text-sm">
            Already have an account?
            <RouterLink to="/register" class="underline"> Sign In </RouterLink>
          </div>
        </CardContent>
      </Card>
    </div>
  </div>
</template>

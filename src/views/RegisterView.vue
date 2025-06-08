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
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group";
import router from "@/router";
import { AlertCircleIcon } from "lucide-vue-next";
import { reactive } from "vue";

const state = reactive({
  form: {
    name: "",
    email: "",
    gender: "Male",
    password: "",
    confirmPassword: "",
  },
  msg: "",
  isLoading: false,
});

const registerHandler = async () => {
  try {
    state.msg = "";
    state.isLoading = true;

    const { password, confirmPassword } = state.form;
    if (password === confirmPassword) {
      await defaultInstance.post("/register", state.form);
      router.push("/login");
    } else {
      state.msg = "Password unmatched";
    }
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
                <Label for="name">Name</Label>
                <Input
                  id="name"
                  v-model="state.form.name"
                  type="text"
                  placeholder="Rafli"
                  required
                />
              </div>
              <div class="grid gap-2">
                <Label for="email">Email</Label>
                <Input
                  id="email"
                  v-model="state.form.email"
                  type="email"
                  placeholder="mido@parasol.com"
                  required
                />
              </div>
              <div class="grid gap-2">
                <Label htmlFor="Gender">Gender</Label>
                <RadioGroup class="flex" v-model="state.form.gender">
                  <div class="flex items-center space-x-2">
                    <RadioGroupItem value="Male" id="male" />
                    <Label htmlFor="male">Male</Label>
                  </div>
                  <div class="flex items-center space-x-2">
                    <RadioGroupItem value="Female" id="female" />
                    <Label htmlFor="female">Female</Label>
                  </div>
                </RadioGroup>
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
              <div class="grid gap-2">
                <Label for="confirmPassword">Confirm Password</Label>
                <Input
                  v-model="state.form.confirmPassword"
                  id="confirmPassword"
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
            <RouterLink to="/login" class="underline"> Sign In </RouterLink>
          </div>
        </CardContent>
      </Card>
    </div>
  </div>
</template>

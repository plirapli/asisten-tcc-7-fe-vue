<script setup lang="ts">
import { reactive, defineProps, ref } from "vue";
import { Loader2, Pencil } from "lucide-vue-next";
import { Button } from "./ui/button";
import {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerFooter,
  DrawerHeader,
  DrawerTitle,
  DrawerTrigger,
} from "./ui/drawer";
import { Label } from "./ui/label";
import { Input } from "./ui/input";
import { RadioGroup, RadioGroupItem } from "./ui/radio-group";
import { strictInstance } from "@/api/instance";

const open = ref(false);
const isLoading = ref(false);
const props = defineProps(["user"]);
const emit = defineEmits(["item-edit"]);

const defaultValue = {
  name: props.user.name,
  email: props.user.email,
  gender: props.user.gender,
};

const form = reactive({
  name: props.user.name,
  email: props.user.email,
  gender: props.user.gender,
});

const editUser = async () => {
  try {
    if (JSON.stringify(defaultValue) !== JSON.stringify(form)) {
      isLoading.value = true;
      await strictInstance.put(`/users/${props.user.id}`, form);
      emit("item-edit", { id: props.user.id, ...form });
    }
    open.value = false;
  } catch (error: any) {
    console.log(error.message);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <Drawer v-model:open="open">
    <DrawerTrigger asChild>
      <Button variant="outline" size="icon" class="cursor-pointer">
        <Pencil />
      </Button>
    </DrawerTrigger>
    <DrawerContent>
      <div class="flex justify-center">
        <div class="w-full max-w-xl">
          <form @submit.prevent="editUser">
            <DrawerHeader>
              <DrawerTitle>Edit User</DrawerTitle>
              <DrawerDescription>
                Ubah data user {{ user.name }}
              </DrawerDescription>
            </DrawerHeader>
            <div class="grid gap-4 p-4 pt-2">
              <div class="grid grid-cols-4 items-center gap-4">
                <Label htmlFor="name" class="text-right"> Name </Label>
                <Input
                  v-model="form.name"
                  required
                  id="name"
                  placeholder="Hehehe"
                  class="col-span-3"
                />
              </div>
              <div class="grid grid-cols-4 items-center gap-4">
                <Label htmlFor="email" class="text-right"> Email </Label>
                <Input
                  v-model="form.email"
                  id="email"
                  type="email"
                  placeholder="hehe@gmail"
                  class="col-span-3"
                />
              </div>
              <div class="grid grid-cols-4 items-center gap-4">
                <Label htmlFor="Gender" class="text-right"> Gender </Label>
                <RadioGroup class="flex" v-model="form.gender">
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
            </div>
            <DrawerFooter>
              <Button
                type="submit"
                class="cursor-pointer"
                :disabled="isLoading"
              >
                Save Changes <Loader2 v-if="isLoading" class="animate-spin" />
              </Button>
              <DrawerClose asChild>
                <Button
                  type="button"
                  variant="outline"
                  class="w-full cursor-pointer"
                >
                  Cancel
                </Button>
              </DrawerClose>
            </DrawerFooter>
          </form>
        </div>
      </div>
    </DrawerContent>
  </Drawer>
</template>

<script setup lang="ts">
import { ref, defineProps } from "vue";
import { strictInstance } from "@/api/instance";
import {
  Dialog,
  DialogClose,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "./ui/dialog";
import { Button } from "./ui/button";
import { Loader2, Trash2 } from "lucide-vue-next";

const open = ref(false);
const isLoading = ref(false);

const props = defineProps(["user"]);
const emit = defineEmits(["item-deleted"]);

const deleteUser = async () => {
  try {
    isLoading.value = true;
    await strictInstance.delete(`/users/${props.user.id}`);
    emit("item-deleted", props.user.id); // Notify parent
    open.value = false;
  } catch (error: any) {
    console.log(error.message);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <Dialog v-model:open="open">
    <DialogTrigger asChild>
      <Button
        variant="outline"
        size="icon"
        class="border-red-500 hover:bg-red-50 transition-all cursor-pointer"
      >
        <Trash2 color="red" />
      </Button>
    </DialogTrigger>
    <DialogContent class="sm:max-w-[425px]">
      <DialogHeader>
        <DialogTitle>Delete User</DialogTitle>
        <DialogDescription>
          Are you sure you want to delete {{ user!.name }}?
        </DialogDescription>
      </DialogHeader>
      <form @submit.prevent="deleteUser">
        <DialogFooter>
          <Button type="submit" class="cursor-pointer" :disabled="isLoading">
            Yes <Loader2 v-if="isLoading" class="animate-spin" />
          </Button>
          <DialogClose asChild>
            <Button type="button" class="cursor-pointer" variant="outline">
              No
            </Button>
          </DialogClose>
        </DialogFooter>
      </form>
    </DialogContent>
  </Dialog>
</template>

<script setup lang="ts">
import { strictInstance } from "@/api/instance";
import { Button } from "@/components/ui/button";
import { Skeleton } from "@/components/ui/skeleton";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";
import UserTable from "@/components/UserTable.vue";
import router from "@/router";
import { LogOutIcon } from "lucide-vue-next";
import { onMounted, ref } from "vue";

interface User {
  id: number;
  name: string;
  email?: string;
  gender?: string;
}

interface EditEvent {
  id: number;
  [key: string]: unknown;
}

const users = ref<User[]>([]);
const msg = ref("");
const isLoading = ref(true);

const handleEdit = (event: EditEvent) => {
  const index = users.value.findIndex(
    (user: { id: number }) => user.id === event.id
  );

  if (index !== -1) {
    users.value[index] = {
      ...users.value[index],
      ...event,
    };
  }
};

const handleItemDeleted = (deletedId: Number) => {
  users.value = users.value.filter((user) => user["id"] !== deletedId);
};

const logoutHandler = async () => {
  try {
    await strictInstance.delete("/logout");
    localStorage.removeItem("accessToken");
    router.replace("/login");
  } catch (error: any) {
    console.error(error?.response?.data || error.message);
  }
};

onMounted(async () => {
  try {
    isLoading.value = true;
    const response = await strictInstance.get("/users");
    users.value = response.data.data;
  } catch (error: any) {
    msg.value = `Error: ${error.message}`;
  } finally {
    isLoading.value = false;
  }
});
</script>

<template>
  <div class="flex justify-center">
    <div class="w-full max-w-xl p-5">
      <div class="mb-2">
        <Button @click="logoutHandler" class="cursor-pointer">
          <LogOutIcon class="w-4" /> Logout
        </Button>
      </div>
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead class="w-[40px] text-center">No.</TableHead>
            <TableHead>Name</TableHead>
            <TableHead>Email</TableHead>
            <TableHead>Gender</TableHead>
            <TableHead class="w-[100px]"></TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          <TableRow v-if="isLoading">
            <TableCell>
              <Skeleton class="w-[40px] h-[20px] rounded-full" />
            </TableCell>
            <TableCell>
              <Skeleton class="h-[20px] rounded-full" />
            </TableCell>
            <TableCell>
              <Skeleton class="h-[20px] rounded-full" />
            </TableCell>
            <TableCell>
              <Skeleton class="h-[20px] rounded-full" />
            </TableCell>
          </TableRow>
          <UserTable
            v-else
            v-for="(user, i) in users"
            :key="user.id"
            :user="user"
            @item-edit="handleEdit"
            @item-deleted="handleItemDeleted"
            :i="i + 1"
          />
        </TableBody>
      </Table>
    </div>
  </div>
</template>

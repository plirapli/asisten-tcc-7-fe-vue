<script setup lang="ts">
import { defineProps } from "vue";
import { TableCell, TableRow } from "./ui/table";
import UserDelete from "./UserDelete.vue";

defineProps({
  user: {
    type: Object,
    default: { id: "", name: "", email: "", gender: "" },
  },
  i: Number,
});
const emit = defineEmits(["item-deleted"]);
</script>

<template>
  <TableRow>
    <TableCell class="text-center">{{ i }}</TableCell>
    <TableCell class="font-medium">{{ user.name }}</TableCell>
    <TableCell>{{ user.email }}</TableCell>
    <TableCell>{{ user.gender }}</TableCell>
    <TableCell>
      <div class="flex gap-1.5">
        <!-- <EditUser setIsGet="{setIsGet}" user="{user}" /> -->
        <UserDelete
          :user="user"
          @item-deleted="$emit('item-deleted', $event)"
        />
      </div>
    </TableCell>
  </TableRow>
</template>

<script setup lang="ts">
import { defineProps } from "vue";
import { TableCell, TableRow } from "./ui/table";

defineProps({
  user: {
    type: Object,
    default: { name: "", email: "", gender: "" },
  },
  i: Number,
});
</script>

<template>
  <TableRow>
    <TableCell class="text-center">{{ i }}</TableCell>
    <TableCell class="font-medium">{{ user.name }}</TableCell>
    <TableCell>{{ user.email }}</TableCell>
    <TableCell>{{ user.gender }}</TableCell>
    <TableCell>
      <div class="flex gap-1.5">
        <!-- <EditUser setIsGet="{setIsGet}" user="{user}" />
        <DeleteUser setIsGet="{setIsGet}" user="{user}" /> -->
      </div>
    </TableCell>
  </TableRow>
</template>

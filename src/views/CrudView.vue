<template>
  <div class="container">
    <TableContacts />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import FilterContact from '@/components/FilterContact.vue';
import TableContacts from '@/components/TableContacts.vue';

export default defineComponent({
  name: 'CrudView',
  components: {
    TableContacts
  },
});
</script>

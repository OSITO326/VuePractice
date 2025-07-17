<template>
    <div class="container mt-4">
      <form @submit.prevent="emitirDatosEditados" novalidate>
        <div class="mb-3">
          <label class="form-label">Name</label>
          <input v-model="editData.name" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">Email</label>
          <input v-model="editData.email" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">Address</label>
          <input v-model="editData.address" type="text" class="form-control" required />
        </div>

        <div class="mb-3">
          <label class="form-label">Phone</label>
          <input v-model="editData.phone" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">Country</label>
          <input v-model="editData.country" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">City</label>
          <input v-model="editData.city" type="text" class="form-control" required />
        </div>
  
        <button type="submit" class="btn btn-success">Guardar Cambios</button>
      </form>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface Contact {
  id: number;
  name: string;
  email: string;
  address: string;
  phone: string;
  country: string;
  city: string;
}

export default defineComponent({
  name: "EditContact",
    props: {
      contact: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        // Copia del objeto original para evitar modificarlo directamente
        editData: { ...this.contact } as Contact
      }
    },
    methods: {
      emitirDatosEditados() {
        // Emitimos un evento con los datos modificados
        this.$emit('update', this.editData)
      }
    }
});
</script>

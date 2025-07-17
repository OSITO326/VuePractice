<template>
  <div class="hello">
    <h1 class="text-start fs-1">{{ title }}</h1>

    <form @submit.prevent="submit">
      <div class="mb-3">
        <label>Name</label>
        <input v-model="newData.name" class="form-control" />
      </div>
      <div class="mb-3">
        <label>Email</label>
        <input v-model="newData.email" class="form-control" />
      </div>
      <div class="mb-3">
        <label>Address</label>
        <input v-model="newData.address" class="form-control" />
      </div>
      <div class="mb-3">
        <label>Phone</label>
        <input v-model="newData.phone" class="form-control" />
      </div>
      <div class="mb-3">
        <label>Country</label>
        <input v-model="newData.country" class="form-control" />
      </div>
      <div class="mb-3">
        <label>City</label>
        <input v-model="newData.city" class="form-control" />
      </div>

      <button type="submit" class="btn btn-success">Guardar</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, PropType, SetupContext } from "vue";

interface Contact {
  id: number;
  name: string;
  email: string;
  address: string;
  phone: string;
  country: string;
  city: string
}

interface Props {
  title: string;
  contact?: Contact;
}

export default defineComponent({
  name: "NewContact",
  props: {
    title: {
      type: String,
      required: true,
    },
    contact: {
      type: Object as PropType<Contact>,
      required: false,
    },
  },
  emits: ["created"],
  setup(props: Props, { emit }: SetupContext<["created"]>) {
    const newData = ref<Contact>({
      id: props.contact?.id || 0,
      name: props.contact?.name || "",
      email: props.contact?.email || "",
      address: props.contact?.address || "",
      phone: props.contact?.phone || "",
      country: props.contact?.country || "",
      city: props.contact?.city || "",
    });

    const submit = () => {
      emit("created", newData.value);
    };

    return {
      newData,
      submit,
    };
  },
});
</script>

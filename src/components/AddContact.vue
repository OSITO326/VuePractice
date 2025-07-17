<template>
  <div>
    <h1>{{ title }}</h1>
    <button class="btn btn-primary mb-3" @click="openModal">Nuevo contacto</button>

    <!-- Modal Bootstrap -->
    <div class="modal fade" id="modalAddContact" tabindex="-1" ref="modalRef">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Crear nuevo contacto</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
          </div>
          <div class="modal-body">
            <NewContact :title="title" @created="handleCreated" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";
import NewContact from "./NewContact.vue";

export default defineComponent({
  name: "AddContact",
  props: {
    title: {
      type: String,
      default: "",
    },
  },
  components: {
    NewContact,
  },
  emits: ["created"], // <-- esto permite emitir el evento al padre
  setup(props, { emit }) {
    const modalRef = ref<HTMLElement | null>(null);
    let modalInstance: any = null;

    onMounted(() => {
      if (modalRef.value) {
        // @ts-ignore
        modalInstance = new bootstrap.Modal(modalRef.value);
      }
    });

    const openModal = () => {
      if (modalInstance) modalInstance.show();
    };

    const handleCreated = (newContact: any) => {
      emit("created", newContact); // <--- se emite al padre (CrudView.vue)
      if (modalInstance) modalInstance.hide();
    };

    return {
      modalRef,
      openModal,
      handleCreated,
    };
  },
});
</script>

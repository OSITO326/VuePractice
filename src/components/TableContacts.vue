<template>
  <div class="mt-3">
    <div class="mt-2 mb-2">
      <FilterContact title="Filtrar contactos"
       description="Busca contactos por nombre o correo electrónico"
       @filter="handleFilter" />
      <AddContact title="Agenda de contactos" @created="agregarNuevo"/>
    </div>
    <table class="table table-bordered border-primary">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Name</th>
          <th scope="col">Email</th>
          <th scope="col">Address</th>
          <th scope="col">Phone</th>
          <th scope="col">Country</th>
          <th scope="col">City</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in getItems" :key="item.id">
          <th scope="row">{{ item.id }}</th>
          <td>{{ item.name }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.address }}</td>
          <td>{{ item.phone }}</td>
          <td>{{ item.country }}</td>
          <td>{{ item.city }}</td>
          <td>
            <button type="button" class="btn btn-primary m-1" @click="openModal(index)">
              Editar
            </button>
            <button type="button" class="btn btn-danger m-1" @click="deleteContact(index)">
              Eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Modal Bootstrap -->
  <div class="modal fade" id="modalAutoEditar" tabindex="-1" aria-labelledby="modalAutoEditarLabel" aria-hidden="true"
      ref="modalRef">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
              <h5 class="modal-title" id="modalAutoEditarLabel">Editar Contacto</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
          </div>
          <div class="modal-body">
              <!-- Componente EditContact -->
              <EditContact v-if="modalMode =='editar' && contactSelected" :contact="contactSelected" @update="guardarEdicion"
                  @cancelar="cerrarModal" />
            <NewContact v-if="modalMode =='crear'" title="Nuevo Contacto" @created="agregarNuevo($event)"></NewContact>
          </div>
        </div>
      </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import EditContact from "./EditContact.vue";
import NewContact from "./NewContact.vue";
import AddContact from "./AddContact.vue";
import FilterContact from "./FilterContact.vue";

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
  name: "TableContacts",
  props: {
  },
  data() {
    return {
      items: [
        { 
          id: 1, 
          name: "Alice Johnson", 
          email: "alice.johnson@example.com", 
          address: "123 Maple Street", 
          phone: "123-456-7890", 
          country: "USA", 
          city: "New York" 
        }, 
        { 
          id: 2, 
          name: "Bob Smith", 
          email: "bob.smith@example.com", 
          address: "456 Oak Avenue", 
          phone: "987-654-3210", 
          country: "Canada", 
          city: "Toronto" 
        }, 
        { 
          id: 3, 
          name: "Carol White", 
          email: "carol.white@example.com",
          address: "789 Pine Road", 
          phone: "555-123-4567", 
          country: "UK", 
          city: "London" 
        }, 
        { 
          id: 4, 
          name: "David Brown", 
          email: "david.brown@example.com",
          address: "321 Elm Street", 
          phone: "444-555-6666", 
          country: "Australia", 
          city: "Sydney" 
        }, 
        { 
          id: 5, 
          name: "Emily Davis", 
          email: "emily.davis@example.com",
          address: "654 Spruce Lane", 
          phone: "333-444-5555", 
          country: "USA", 
          city: "Los Angeles" 
        } 
      ] as Contact[],
      modalBootstrapInstance: null as any,
      modalMode: "editar", // o "crear" según el caso
      contactSelected: null as Contact | null,
      indexSelected: null as number | null,
      filterText: "",
    };
  },
  components: {
    EditContact,
    NewContact,
    AddContact,
    FilterContact,
  },
  methods: {
    goToNew() {
        this.modalMode="crear";
        if (!this.modalBootstrapInstance) {
            // @ts-ignore
            this.modalBootstrapInstance = new (window as any).bootstrap.Modal(this.$refs.modalRef);
        }
        if (this.modalBootstrapInstance) {
            this.modalBootstrapInstance.show();
        } else {
            console.error('modalBootstrapInstance no está inicializado');
        }
    },
    openModal(index: number) {
      this.indexSelected = index;
      this.contactSelected = this.items[index]; // Asegura que pase el objeto a EditContact
      this.modalMode = "editar";

      if (!this.modalBootstrapInstance) {
        // @ts-ignore
        this.modalBootstrapInstance = new (window as any).bootstrap.Modal(this.$refs.modalRef);
      }

      if (this.modalBootstrapInstance) {
        this.modalBootstrapInstance.show(); // ✅ Mostrar el modal
      } else {
        console.error('modalBootstrapInstance no está inicializado');
      }
    },
    cerrarModal() {
        if (this.modalBootstrapInstance) {
            this.modalBootstrapInstance.hide();
        }
    },
    guardarEdicion(contactEdit: Contact) {
        console.log('Auto editado:', contactEdit);
        // Aquí actualizas la info, haces llamada a API, etc.
        if (this.indexSelected !== null) {
          this.items[this.indexSelected] = contactEdit;
        }
        this.cerrarModal();
    },
    deleteContact(index: number) {
        if (confirm('¿Está seguro de eliminar este contacto?')) {
            this.items.splice(index, 1);
        }
    },
    agregarNuevo($event: Contact){
      const maxId = Math.max(...this.items.map(contact => contact.id), 0);
      $event.id = maxId + 1;
      this.items.push($event);
      this.cerrarModal();
    },
    // filtro de busqueda name && email, show all if empty input
    handleFilter(searchText: string) {
      this.filterText = searchText;
      if (this.filterText.trim() === "") {
        this.getItems = this.items; // Reset to original items if filter is empty
      } else {
        this.getItems = this.items.filter((contact) => {
          const text = this.filterText.toLowerCase();
          return (
            contact.name.toLowerCase().includes(text) ||
            contact.email.toLowerCase().includes(text)
          );
        });
      }
      this.$emit("filter", this.filterText); // Emit the filter event if needed
    },
  },
  computed: {
    getItems(): Contact[] {
      if (!this.filterText || this.filterText.trim() === "") {
        return this.items;
      }
      return this.items.filter((contact) => {
        const text = this.filterText.toLowerCase();
        return (
          contact.name.toLowerCase().includes(text) ||
          contact.email.toLowerCase().includes(text)
        );
      });
    },
  },
  setup() {
    const newContact = ref(null);

    const addContactToTable = (contact: any) => {
      newContact.value = contact;
    };

    return {
      newContact,
      addContactToTable
    };
  },
});
</script>

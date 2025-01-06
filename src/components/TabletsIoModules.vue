<template>
  <v-data-table
    v-model:selected="selected"
    :headers="headers"
    :items="modules"
    :search="search"
    item-value="id"
    style="height: calc(100vh)"
    show-select
    multi-sort
    :loading="loading"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          density="compact"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          variant="solo-filled"
          single-line
          flat
          hide-details
          style="width: 100%"
        ></v-text-field>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog">
          <template v-slot:activator="{ props }">
            <v-btn variant="tonal" color="primary" dark v-bind="props">
              New IO Module
            </v-btn>
            <v-divider class="mx-4" inset vertical></v-divider>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['module_type_name'].String"
                      label="Module Type Name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="editedItem['module_type_number'].Int32"
                      label="Module Type Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['manufacturer_bottom_qr_code'].String"
                      label="Bottom QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['manufacturer_top_qr_code'].String"
                      label="Top QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['rma_number'].String"
                      label="RMA Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="editedItem['hw_version'].Int32"
                      label="HW Version"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['invoice_number'].String"
                      label="Invoice Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['customer_name'].String"
                      label="Customer Name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['controllers_id']"
                      label="Controller ID"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="
                        editedItem['controller slot number'].Int32
                      "
                      label="Controller Slot"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-textarea
                      v-model="formattedPinout"
                      label="Pinout JSON"
                      auto-grow
                      rows="4"
                    ></v-textarea>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="close">
                Cancel
              </v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="save">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon size="small" class="me-2" @click="editItem(item)"
        >mdi-pencil</v-icon
      >
      <v-icon size="small" @click="deleteItem(item)">mdi-delete</v-icon>
    </template>
  </v-data-table>
</template>

<script>
import { ref, reactive, computed, onMounted } from "vue";

export default {
  setup() {
    const search = ref("");
    const selected = ref([]);
    const dialog = ref(false);
    const modules = ref([]);
    const editedIndex = ref(-1);
    const loading = ref(true);
    const editedItem = reactive({
      id: "",
      module_type_name: { String: "", Valid: true },
      module_type_number: { Int32: 0, Valid: true },
      manufacturer_bottom_qr_code: { String: "", Valid: true },
      manufacturer_top_qr_code: { String: "", Valid: true },
      rma_number: { String: "", Valid: true },
      order_id: null,
      hw_version: { Int32: 0, Valid: true },
      invoice_number: { String: "", Valid: false },
      customer_name: { String: "", Valid: false },
      controllers_id: "",
      "controller slot number": { Int32: 0, Valid: true },
      pinout_json: {
        RawMessage: {},
        Valid: true,
      },
    });
    const defaultItem = { ...editedItem };
    const formattedPinout = computed(() =>
      JSON.stringify(editedItem.pinout_json.RawMessage, null, 2),
    );

    const headers = [
      { title: "Module Type", key: "module_type_name.String" },
      { title: "Type Number", key: "module_type_number.Int32" },
      { title: "Bottom QR", key: "manufacturer_bottom_qr_code.String" },
      { title: "Top QR", key: "manufacturer_top_qr_code.String" },
      { title: "RMA Number", key: "rma_number.String" },
      { title: "Order ID", key: "order_id" },
      { title: "HW Version", key: "hw_version.Int32" },
      { title: "Invoice Number", key: "invoice_number.String" },
      { title: "Customer Name", key: "name.String" },
      { title: "Controller ID", key: "controllers_id" },
      { title: "Controller Slot", key: "controller slot number.Int32" },
      {
        title: "Pinout",
        key: "pinout_json.RawMessage",
        sortable: false,
        nowrap: true,
      },
      { title: "Actions", key: "actions", sortable: false },
    ];

    const formTitle = computed(() =>
      editedIndex.value === -1 ? "New Module" : "Edit Module",
    );

    async function fetchModules() {
      loading.value = true;
      try {
        const response = await fetch("http://localhost:8081/api/iomodules");
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        modules.value = data;
      } catch (error) {
        console.error("Error fetching modules:", error);
        // You might want to show an error message to the user here
      } finally {
        loading.value = false;
      }
    }

    function editItem(item) {
      editedIndex.value = modules.value.indexOf(item);
      Object.assign(editedItem, item);
      dialog.value = true;
    }

    function deleteItem(item) {
      const index = modules.value.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        modules.value.splice(index, 1);
    }

    function close() {
      dialog.value = false;
      editedIndex.value = -1;
      Object.assign(editedItem, defaultItem);
    }

    function save() {
      if (editedIndex.value > -1) {
        Object.assign(modules.value[editedIndex.value], editedItem);
      } else {
        modules.value.push({ ...editedItem, id: crypto.randomUUID() });
      }
      close();
    }

    onMounted(() => {
      fetchModules();
    });

    return {
      search,
      selected,
      dialog,
      modules,
      editedIndex,
      editedItem,
      defaultItem,
      headers,
      formTitle,
      loading,
      editItem,
      deleteItem,
      close,
      save,
      formattedPinout,
    };
  },
};
</script>

<style scoped></style>

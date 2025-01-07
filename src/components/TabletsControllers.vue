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
                      v-model="editedItem['controller_type_name'].String"
                      label="Controller Type"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['pcb_hw_version']"
                      label="PCB HW Version"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['manufacturer_name'].String"
                      label="Manufacturer"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['manufacturer_qr_code'].String"
                      label="Manufacturer QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['serial_number'].String"
                      label="Serial Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['mac_address'].String"
                      label="MAC Address"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['sim_number'].String"
                      label="SIM Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['mini_pcie_modules_name'].String"
                      label="Mini PCIe Modules"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['mini_pcie_serial_number'].String"
                      label="Mini PCIe Serial Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['m2_module_name'].String"
                      label="M.2 Module"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model.number="editedItem['m2_module_type_number'].Int32"
                      label="M.2 Module Type Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['article_number'].String"
                      label="Article Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['qr_code'].String"
                      label="QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['project_name'].String"
                      label="Project Name"
                    ></v-text-field>
                  </v-col>
                  <v-row cols="12" class="pa-3" no-gutters>
                    <v-col cols="12" align-self="center" class="text-subtitle-1">
                      CAN termination
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_1_terminated'].Bool"
                        label="CAN 1 terminated"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_2_terminated'].Bool"
                        label="CAN 2 terminated"
                        dense
                      ></v-checkbox>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_3_terminated'].Bool"
                        label="CAN 3 terminated"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_4_terminated'].Bool"
                        label="CAN 4 terminated"
                        dense
                      ></v-checkbox>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['usb'].Bool"
                        label="USB"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['serial'].Bool"
                        label="Serial"
                        dense
                      ></v-checkbox>
                    </v-col>
                  </v-row>
                  <v-col cols="12">
                    <v-textarea
                      v-model="editedItem['description'].String"
                      label="Description"
                      auto-grow
                      rows="4"
                    ></v-textarea>
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
      controller_type_name: { String: "", Valid: true },
      io_module_socket_amount: { Int32: 0, Valid: true },
      project_name: { String: "", Valid: true },
      description: { String: "", Valid: true },
      pcb_hw_version: "",
      pcb_version_number: { Int32: 0, Valid: true },
      serial_number: { String: "", Valid: true },
      manufacturer_name: { String: "", Valid: true },
      assembly_date: { Time: "0001-01-01T00:00:00Z", Valid: false },
      mac_address: { String: "", Valid: true },
      sim_number: { String: "", Valid: true },
      mini_pcie_modules_name: { String: "", Valid: true },
      mini_pcie_serial_number: { String: "", Valid: true },
      m2_module_name: { String: "", Valid: true },
      m2_module_type_number: { Int32: 0, Valid: true },
      display_adapters_id: null,
      article_number: { String: "", Valid: true },
      qr_code: { String: "", Valid: true },
      can_1_terminated: { Bool: false, Valid: true },
      can_2_terminated: { Bool: false, Valid: true },
      can_3_terminated: { Bool: false, Valid: true },
      can_4_terminated: { Bool: false, Valid: true },
      usb: { Bool: false, Valid: true },
      serial: { Bool: false, Valid: true },
      manufacturer_qr_code: { String: "", Valid: true },
      order_id: "",
      slot_pinout_json: {
        RawMessage: {},
        Valid: true,
      },
    });
    const defaultItem = { ...editedItem };
    const formattedPinout = computed(() =>
      JSON.stringify(editedItem.slot_pinout_json.RawMessage, null, 2),
    );

    const headers = [
      { title: "ID", key: "id" },
      { title: "Controller Type", key: "controller_type_name.String" },
      { title: "IO Module Sockets", key: "io_module_socket_amount.Int32" },
      { title: "Project", key: "project_name.String" },
      { title: "Description", key: "description.String" },
      { title: "PCB HW Version", key: "pcb_hw_version" },
      { title: "PCB Version", key: "pcb_version_number.Int32" },
      { title: "Serial Number", key: "serial_number.String" },
      { title: "Manufacturer", key: "manufacturer_name.String" },
      { title: "Assembly Date", key: "assembly_date.Time" },
      { title: "MAC Address", key: "mac_address.String" },
      { title: "SIM Number", key: "sim_number.String" },
      { title: "Mini PCIe Module", key: "mini_pcie_modules_name.String" },
      { title: "Mini PCIe Serial", key: "mini_pcie_serial_number.String" },
      { title: "M2 Module", key: "m2_module_name.String" },
      { title: "M2 Module Type", key: "m2_module_type_number.Int32" },
      { title: "Display Adapter ID", key: "display_adapters_id" },
      { title: "Article Number", key: "article_number.String" },
      { title: "QR Code", key: "qr_code.String" },
      { title: "CAN 1", key: "can_1_terminated.Bool" },
      { title: "CAN 2", key: "can_2_terminated.Bool" },
      { title: "CAN 3", key: "can_3_terminated.Bool" },
      { title: "CAN 4", key: "can_4_terminated.Bool" },
      { title: "USB", key: "usb.Bool" },
      { title: "Serial", key: "serial.Bool" },
      { title: "Manufacturer QR", key: "manufacturer_qr_code.String" },
      { title: "Order ID", key: "order_id" },
      { title: "Created At", key: "created_at.Time" },
      { title: "Updated At", key: "updated_at.Time" },
      { title: "Is Deleted", key: "is_deleted.Bool" },
      {
        title: "Pinout",
        key: "slot_pinout_json.RawMessage",
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
        const response = await fetch("http://localhost:8081/api/controllers");
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

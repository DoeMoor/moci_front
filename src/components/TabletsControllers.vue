<template>
  <v-data-table
    v-model:selected="selected"
    :headers="headers"
    :items="controllers"
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
              New controller
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
                    <v-autocomplete
                      v-model="editedItem['controller_type_name']"
                      label="Controller Type"
                      :items="controllerTypes"
                      item-title="name"
                      item-value="name"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['pcb_hw_version']"
                      label="PCB HW Version"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      v-model="editedItem['manufacturer_name']"
                      label="Manufacturer"
                      :items="manufacturers"
                      item-title="name.String"
                      item-value="name.String"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['manufacturer_qr_code']"
                      label="Manufacturer QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['serial_number']"
                      label="Serial Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['mac_address']"
                      label="MAC Address"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['sim_number']"
                      label="SIM Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      v-model="editedItem['mini_pcie_modules_name']"
                      label="Mini PCIe module"
                      :items="miniPcieModulesTypes"
                      :item-title="
                        (item) => `${item.name} ${item.moduleTypeNumber}`
                      "
                      :item-value="
                        (item) => `${item.name} ${item.moduleTypeNumber}`
                      "
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['mini_pcie_serial_number']"
                      label="Mini PCIe Serial Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      :items="m2ModuleTypes"
                      v-model="editedItem['m2_module_name']"
                      label="M.2 Module"
                      :item-title="
                        (item) => `${item.name} ${item.moduleTypeNumber}`
                      "
                      :item-value="
                        (item) => `${item.name} ${item.moduleTypeNumber}`
                      "
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['LED_board']"
                      label="LED board qr code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['article_number']"
                      label="Article Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['qr_code']"
                      label="QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem['project_name']"
                      label="Project Name"
                    ></v-text-field>
                  </v-col>
                  <v-row cols="12" class="pa-3">
                    <v-col cols="12" align-self="center" class="text-h6">
                      CAN termination
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_1_terminated']"
                        label="CAN 1 terminated"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_2_terminated']"
                        label="CAN 2 terminated"
                        dense
                      ></v-checkbox>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_3_terminated']"
                        label="CAN 3 terminated"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can_4_terminated']"
                        label="CAN 4 terminated"
                        dense
                      ></v-checkbox>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['usb']"
                        label="USB"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['serial']"
                        label="Serial"
                        dense
                      ></v-checkbox>
                    </v-col>
                  </v-row>
                  <v-col cols="12">
                    <v-textarea
                      v-model="editedItem['description']"
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
    const miniPcieModulesTypes = ref([]);
    const manufacturers = ref([]);
    const controllerTypes = ref([]);
    const m2ModuleTypes = ref([]);

    const search = ref("");
    const selected = ref([]);
    const dialog = ref(false);
    const controllers = ref([]);
    const editedIndex = ref(-1);
    const loading = ref(true);
    const editedItem = reactive({
      id: null,
      controller_type_name: null,
      io_module_socket_amount: null,
      project_name: null,
      description: null,
      pcb_hw_version: null,
      pcb_version_number: null,
      serial_number: null,
      manufacturer_name: null,
      assembly_date: null,
      mac_address: null,
      sim_number: null,
      mini_pcie_modules_name: null,
      mini_pcie_serial_number: null,
      m2_module_name: null,
      m2_module_type_number: null,
      LED_board: null,
      display_adapters_id: null,
      article_number: null,
      qr_code: null,
      can_1_terminated: null,
      can_2_terminated: null,
      can_3_terminated: null,
      can_4_terminated: null,
      usb: null,
      serial: null,
      manufacturer_qr_code: null,
      order_id: null,
      slot_pinout_json: {
        RawMessage: {},
      },
    });
    const defaultItem = { ...editedItem };

    const formattedPinout = computed(() =>
      JSON.stringify(editedItem.slot_pinout_json.RawMessage, null, 2),
    );

    const headers = [
      { title: "Controller Type", key: "ControllerTypeName" },
      { title: "PCB HW Version", key: "PcbHwVersion" },
      { title: "Controller Serial Number", key: "ControllerSerialNumber" },
      { title: "Project", key: "ProjectName" },
      { title: "Description", key: "Description" },
      { title: "Article Number", key: "ArticleNumber" },
      { title: "Display Adapter Type", key: "DisplayType" },
      { title: "Assembly Date", key: "AssemblyDate" },
      { title: "Order ID", key: "OrderID" },
      { title: "Controller Manufacturer QR", key: "ControllerManufacturerQrCodes" },
      { title: "Actions", key: "actions", sortable: false },
    ];

    const formTitle = computed(() =>
      editedIndex.value === -1 ? "New Controller" : "Edit Controller",
    );

    async function fetchControllers() {
      loading.value = true;
      try {
        const response = await fetch("http://localhost:8081/api/controllers");
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        controllers.value = data;
      } catch (error) {
        console.error("Error fetching controllers:", error);
        // show an error message to the user here or not..:(
      } finally {
        loading.value = false;
      }
    }

    async function fetchAllControllerTypes() {
      try {
        const response = await fetch(
          "http://localhost:8081/api/controllers/types",
        );
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        controllerTypes.value = data;
      } catch (error) {
        console.error("Error fetching ControllerTypes:", error);
      }
    }

    async function fetchAllManufacturers() {
      try {
        const response = await fetch("http://localhost:8081/api/manufacturers");
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        manufacturers.value = data;
      } catch (error) {
        console.error("Error fetching Manufacturers:", error);
      }
    }

    async function fetchAllM2ModulesTypes() {
      try {
        const response = await fetch(
          "http://localhost:8081/api/m2Modules/types",
        );
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        m2ModuleTypes.value = data;
      } catch (error) {
        console.error("Error fetching m2ModuleTypes:", error);
      }
    }

    async function fetchAllPCIeModulesTypes() {
      try {
        const response = await fetch(
          "http://localhost:8081/api/miniPcieModules/types",
        );
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        miniPcieModulesTypes.value = data;
      } catch (error) {
        console.error("Error fetching miniPcieModulesTypes:", error);
      }
    }

    function editItem(item) {
      editedIndex.value = controllers.value.indexOf(item);
      Object.assign(editedItem, item);
      dialog.value = true;
    }

    function deleteItem(item) {
      const index = controllers.value.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        controllers.value.splice(index, 1);
    }

    function close() {
      dialog.value = false;
      editedIndex.value = -1;
      Object.assign(editedItem, defaultItem);
    }

    function save() {
      if (editedIndex.value > -1) {
        Object.assign(controllers.value[editedIndex.value], editedItem);
      } else {
        controllers.value.push({ ...editedItem, id: crypto.randomUUID() });
      }
      close();
    }

    onMounted(() => {
      fetchControllers();
      fetchAllControllerTypes();
      fetchAllManufacturers();
      fetchAllPCIeModulesTypes();
      fetchAllM2ModulesTypes();
    });

    return {
      search,
      selected,
      dialog,
      controllers,
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

      controllerTypes,
      manufacturers,
      miniPcieModulesTypes,
      m2ModuleTypes,
    };
  },
};
</script>

<style scoped></style>

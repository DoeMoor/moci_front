<template>
  <v-data-table
    v-model="selected"
    :headers="headers"
    :items="controllers"
    :search="search"
    item-value="id"
    style="height: calc(100vh)"
    show-select
    hover
    sticky
    multi-sort
    :loading="loading"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-text-field
          clearable
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
                      clearable
                      v-model="editedItem['controllerTypeName']"
                      label="Controller Type"
                      :items="controllerTypes"
                      item-title="name"
                      item-value="name"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      clearable
                      v-model="editedItem['pcbHwVersion']"
                      label="PCB HW Version"
                      :items="controllersPcbHwVersions"
                      item-title="fullVersion"
                      item-value="fullVersion"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      clearable
                      v-model="editedItem['manufacturerName']"
                      label="Manufacturer"
                      :items="manufacturers"
                      item-title="name.String"
                      item-value="name.String"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['controllerManufacturerQrCodes']"
                      label="Manufacturer QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['controllerSerialNumber']"
                      label="Serial Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['macAddress']"
                      label="MAC Address"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['simNumber']"
                      label="SIM Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      clearable
                      v-model="editedItem['miniPcieModulesName']"
                      label="Mini PCIe module"
                      :items="miniPcieModulesTypes"
                      item-title="fullName"
                      item-value="fullName"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['miniPcieSerialNumber']"
                      label="Mini PCIe Serial Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-autocomplete
                      :items="m2ModuleTypes"
                      v-model="editedItem['m2ModuleName']"
                      label="M.2 Module"
                      item-title="fullName"
                      item-value="fullName"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['ledBoard']"
                      label="LED board qr code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['articleNumber']"
                      label="Article Number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['controllerInfoQrCode']"
                      label="QR Code"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      clearable
                      v-model="editedItem['projectName']"
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
                        v-model="editedItem['can1Terminated']"
                        label="CAN 1 terminated"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can2Terminated']"
                        label="CAN 2 terminated"
                        dense
                      ></v-checkbox>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can3Terminated']"
                        label="CAN 3 terminated"
                        dense
                      ></v-checkbox>
                      <v-checkbox
                        hide-details
                        v-model="editedItem['can4Terminated']"
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
    const controllersPcbHwVersions = ref([]);

    const search = ref("");
    const selected = ref([]);
    const dialog = ref(false);
    const controllers = ref([]);
    const editedIndex = ref(-1);
    const loading = ref(true);
    const editedItem = reactive({
      id: null,
      controllerTypeName: null,
      ioModuleSocketAmount: null,
      projectName: null,
      description: null,
      pcbHwVersion: null,
      pcbVersionNumber: null,
      controllerSerialNumber: null,
      manufacturerName: null,
      assemblyDate: null,
      macAddress: null,
      simNumber: null,
      miniPcieModulesName: null,
      miniPcieModuleTypeNumber: null,
      miniPcieSerialNumber: null,
      m2ModuleName: null,
      m2ModuleTypeNumber: null,
      ledBoard: null,
      displayType: null,
      displayManufacturerQrCode: null,
      articleNumber: null,
      controllerInfoQrCode: null,
      can1Terminated: null,
      can2Terminated: null,
      can3Terminated: null,
      can4Terminated: null,
      usb: null,
      serial: null,
      controllerManufacturerQrCodes: null,
      orderId: null,
      slotPinoutJson: {
        RawMessage: {},
      },
    });
    const defaultItem = { ...editedItem };

    const formattedPinout = computed(() =>
      JSON.stringify(editedItem.slotPinoutJson.RawMessage, null, 2),
    );

    const headers = [
      { title: "Controller Type", key: "controllerTypeName" },
      { title: "PCB HW Version", key: "pcbHwVersion" },
      { title: "Controller Serial Number", key: "controllerSerialNumber" },
      { title: "Project", key: "projectName" },
      { title: "Description", key: "description" },
      { title: "Article Number", key: "articleNumber" },
      { title: "Display Adapter Type", key: "displayType" },
      { title: "Assembly Date", key: "assemblyDate" },
      { title: "Order ID", key: "orderID" },
      {
        title: "Controller Manufacturer QR",
        key: "controllerManufacturerQrCodes",
      },
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

    async function fetchAllControllersPcbHwVersions() {
      try {
        const response = await fetch(
          "http://localhost:8081/api/controllers/pcbHwVersions",
        );
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        controllersPcbHwVersions.value = data;
      } catch (error) {
        console.error("Error fetching controllersPcbHwVersions:", error);
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
      fetchAllControllersPcbHwVersions();
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
      controllersPcbHwVersions,
    };
  },
};
</script>

<style scoped></style>

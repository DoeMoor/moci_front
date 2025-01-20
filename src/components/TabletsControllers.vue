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
    <template #top>
      <v-toolbar flat>
        <v-divider
          class="mx-4"
          inset
          vertical
        />
        <v-spacer />
        <v-text-field
          v-model="search"
          clearable
          density="compact"
          label="Search"
          prepend-inner-icon="mdi-magnify"
          variant="solo-filled"
          single-line
          flat
          hide-details
          style="width: 100%"
        />
        <v-divider
          class="mx-4"
          inset
          vertical
        />
        <v-spacer />
        <v-dialog v-model="dialog">
          <template #activator="{ props }">
            <v-btn
              variant="tonal"
              color="primary"
              dark
              v-bind="props"
            >
              New controller
            </v-btn>
            <v-divider
              class="mx-4"
              inset
              vertical
            />
          </template>
          <v-card
            class="d-flex flex-column"
            :loading="loading"
          >
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text class="flex-grow-1 overflow-y-auto">
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-autocomplete
                      v-model="editedItem['controllerTypeName']"
                      clearable
                      label="Controller Type"
                      :items="controllerTypes"
                      item-title="name"
                      item-value="name"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-autocomplete
                      v-model="editedItem['pcbHwVersion']"
                      clearable
                      label="PCB HW Version"
                      :items="controllersPcbHwVersions"
                      item-title="fullVersion"
                      item-value="fullVersion"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-autocomplete
                      v-model="editedItem['manufacturerName']"
                      clearable
                      label="Manufacturer"
                      :items="manufacturers"
                      item-title="name.String"
                      item-value="name.String"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['controllerManufacturerQrCodes']"
                      clearable
                      label="Manufacturer QR Code"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['controllerSerialNumber']"
                      clearable
                      label="Serial Number"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['macAddress']"
                      clearable
                      label="MAC Address"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['encloserSerialNumber']"
                      clearable
                      label="Encloser Serial Number"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-autocomplete
                      v-model="editedItem['encloserManufacturerName']"
                      clearable
                      :items="manufacturers"
                      item-title="name.String"
                      item-value="name.String"
                      label="Encloser Manufacturer Name"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['simNumber']"
                      clearable
                      label="SIM Number"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-autocomplete
                      v-model="editedItem['miniPcieModulesName']"
                      clearable
                      label="Mini PCIe module"
                      :items="miniPcieModulesTypes"
                      item-title="fullName"
                      item-value="fullName"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['miniPcieSerialNumber']"
                      clearable
                      label="Mini PCIe Serial Number"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-autocomplete
                      v-model="editedItem['m2ModuleName']"
                      :items="m2ModuleTypes"
                      label="M.2 Module"
                      item-title="fullName"
                      item-value="fullName"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['ledBoard']"
                      clearable
                      label="LED board qr code"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['articleNumber']"
                      clearable
                      label="Article Number"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['controllerInfoQrCode']"
                      clearable
                      label="QR Code"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem['projectName']"
                      clearable
                      label="Project Name"
                    />
                  </v-col>
                  <v-row
                    cols="12"
                    class="pa-3"
                  >
                    <v-col
                      cols="12"
                      align-self="center"
                      class="text-h6"
                    >
                      CAN termination
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-checkbox
                        v-model="editedItem['can1Terminated']"
                        hide-details
                        label="CAN 1 terminated"
                        dense
                      />
                      <v-checkbox
                        v-model="editedItem['can2Terminated']"
                        hide-details
                        label="CAN 2 terminated"
                        dense
                      />
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-checkbox
                        v-model="editedItem['can3Terminated']"
                        hide-details
                        label="CAN 3 terminated"
                        dense
                      />
                      <v-checkbox
                        v-model="editedItem['can4Terminated']"
                        hide-details
                        label="CAN 4 terminated"
                        dense
                      />
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-radio-group
                        v-model="connectionType"
                        label="Connection type"
                      >
                        <v-radio
                          label="USB"
                          :value="'usb'"
                        />
                        <v-radio
                          label="Serial"
                          :value="'serial'"
                        />
                      </v-radio-group>
                    </v-col>
                  </v-row>
                  <v-col cols="12">
                    <v-textarea
                      v-model="editedItem['description']"
                      label="Description"
                      auto-grow
                      rows="4"
                    />
                  </v-col>
                  <v-col cols="12">
                    <v-expansion-panels>
                      <v-expansion-panel>
                        <v-expansion-panel-title class="bg-grey-darken-3">
                          PINOUT
                        </v-expansion-panel-title>
                        <v-expansion-panel-text>
                          <!--TODO: add highlight -->
                          <v-textarea
                            v-model="formattedPinout"
                            label="Pinout JSON"
                            readonly
                            auto-grow
                            rows="4"
                          />
                        </v-expansion-panel-text>
                      </v-expansion-panel>
                    </v-expansion-panels>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer />
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="saveAndContinue"
              >
                Save and continue
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template #[`item.actions`]="{ item }">
      <v-icon
        size="small"
        class="me-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        size="small"
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
  </v-data-table>
</template>

<script>
import { ref, reactive, computed, onMounted, watch } from "vue";

export default {
  setup() {
    const miniPcieModulesTypes = ref([]);
    const manufacturers = ref([]);
    const controllerTypes = ref([]);
    const m2ModuleTypes = ref([]);
    const controllersPcbHwVersions = ref([]);

    const pinoutID = ref(null);
    const typePinout = ref(null);
    const formattedPinout = ref("");
    const connectionType = ref("");
    const isFormDataLoaded = ref(false);

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
      encloserSerialNumber: null,
      encloserManufacturerName: null,
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
      slotPinoutJson: {},
    });
    const defaultItem = { ...editedItem };

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

    watch(connectionType, connectionTypeSelection);
    function connectionTypeSelection() {
      if (connectionType.value === "usb") {
        editedItem.usb = true;
        editedItem.serial = false;
      } else if (connectionType.value === "serial") {
        editedItem.usb = false;
        editedItem.serial = true;
      }
    }

    watch(() => editedItem.controllerTypeName, fetchControllersTypePinout);
    async function fetchControllersTypePinout() {
      try {
        pinoutID.value = controllerTypes.value.find(
          (type) => type.name === editedItem.controllerTypeName,
        )?.id;

        if (!pinoutID.value) {
          updateFormattedPinout();
          return null;
        }

        const response = await fetch(
          `http://localhost:8081/api/controllers/types/pinout/${pinoutID.value}`,
        );
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        typePinout.value = data;
        updateFormattedPinout();
      } catch (error) {
        console.error("Error fetching ControllerTypesPinout:", error);
      }
    }
    const updateFormattedPinout = () => {
      formattedPinout.value = typePinout.value
        ? JSON.stringify(typePinout.value, null, 2)
        : "";
      typePinout.value = null;
    };

    async function fetchAllControllers() {
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
      pinoutID.value = null;
      typePinout.value = null;
      formattedPinout.value = "";
      connectionType.value = "";
    }

    function save() {
      if (editedIndex.value > -1) {
        Object.assign(controllers.value[editedIndex.value], editedItem);
      } else {
        controllers.value.push({ ...editedItem, id: crypto.randomUUID() });
      }
      close();
    }

    function saveAndContinue() {
      if (editedIndex.value > -1) {
        Object.assign(controllers.value[editedIndex.value], editedItem);
      } else {
        controllers.value.push({ ...editedItem, id: crypto.randomUUID() });
      }
    }

    watch(dialog, async (newValue) => {
      if (newValue && !isFormDataLoaded.value) {
        try {
          await Promise.all([
            fetchAllControllerTypes(),
            fetchAllManufacturers(),
            fetchAllPCIeModulesTypes(),
            fetchAllM2ModulesTypes(),
            fetchAllControllersPcbHwVersions(),
          ]);
          isFormDataLoaded.value = true;
        } catch (error) {
          console.error("Error loading form data:", error);
        }
      }
    });

    onMounted(() => {
      fetchAllControllers();
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
      saveAndContinue,

      formattedPinout,

      controllerTypes,
      manufacturers,
      miniPcieModulesTypes,
      m2ModuleTypes,
      controllersPcbHwVersions,
      pinoutID,
      typePinout,
      connectionType,
      isFormDataLoaded,
    };
  },
};
</script>

<style scoped></style>

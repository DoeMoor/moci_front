<template><v-data-table v-model="selected" :headers="headers" :items="controllers" :search="search" item-value="id"
  style="height: calc(100vh)" show-select hover sticky multi-sort :loading="loading">
  <template #top>
    <v-toolbar flat>
      <v-divider class="mx-4" inset vertical />
      <v-spacer />
      <v-text-field v-model="search" clearable density="compact" label="Search" prepend-inner-icon="mdi-magnify"
        variant="solo-filled" single-line flat hide-details style="width: 100%" />
      <v-divider class="mx-4" inset vertical />
      <v-spacer />
      <v-dialog v-model="dialog">
        <template #activator="{ props }">
          <v-btn variant="tonal" color="primary" dark v-bind="props" :loading="loading">
            New controller
          </v-btn>
          <v-divider class="mx-4" inset vertical />
        </template>
        <v-card class="d-flex flex-column" :loading="loading">
          <v-card-title>
            <span class="text-h5">{{ formTitle }}</span>
          </v-card-title>
          <v-card-text class="flex-grow-1 overflow-y-auto">
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.typesId" clearable label="Controller type"
                    :items="controllerTypes" item-title="name" item-value="id" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.pcbHwVersionsId" clearable label="Controller PCB HW version"
                    :items="controllersPcbHwVersions" item-title="fullVersion" item-value="id" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.manufacturersId" clearable label="Controller manufacturer"
                    :items="manufacturers" item-title="name" item-value="id" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.manufacturerQrCode" clearable
                    label="Controller Manufacturer QR code" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.serialNumber" clearable label="Controller Serial number" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.macAddress" clearable label="Controller MAC address" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.encloserSerialNumber" clearable label="Encloser serial number" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.encloserManufacturerId" clearable :items="manufacturers"
                    item-title="name" item-value="id" label="Encloser manufacturer name" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.simNumber" clearable label="Controller SIM number" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.miniPcieTypeId" clearable label="Mini PCIe module"
                    :items="miniPcieModulesTypes" item-title="fullName" item-value="id" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.miniPcieSerialNumber" clearable label="Mini PCIe serial number" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.m2ModulesTypesId" :items="m2ModuleTypes" label="M.2 module"
                    item-title="fullName" item-value="id" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.ledBoardQrCode" clearable label="LED board QR code" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.displayTypeName" clearable label="Display type"
                    :items="displayTypes" item-title="name" item-value="name" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.displayManufacturerQrCode" clearable
                    label="Display manufacturer QR code" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.articleNumber" clearable label="Controller article number" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field v-model="editedItem.infoQrCode" clearable label="Controller Info QR code" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.projectsId" :items="projects" clearable label="Project name"
                    item-title="name" item-value="id" />
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-autocomplete v-model="editedItem.customerId" :items="customers" clearable label="Customer name"
                    item-title="name" item-value="id" />
                </v-col>
                <v-row cols="12" class="pa-3">
                  <v-col cols="12" align-self="center" class="text-h6">
                    CAN termination
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-checkbox v-model="editedItem.can1Terminated" hide-details label="CAN 1 terminated" dense />
                    <v-checkbox v-model="editedItem.can2Terminated" hide-details label="CAN 2 terminated" dense />
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-checkbox v-model="editedItem.can3Terminated" hide-details label="CAN 3 terminated" dense />
                    <v-checkbox v-model="editedItem.can4Terminated" hide-details label="CAN 4 terminated" dense />
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-radio-group v-model="connectionTypes" label="Connection type">
                      <v-radio label="USB" :value="'usb'" />
                      <v-radio label="Serial" :value="'serial'" />
                    </v-radio-group>
                  </v-col>
                </v-row>
                <v-col cols="12">
                  <v-textarea v-model="editedItem.description" label="Description" auto-grow rows="4" />
                </v-col>
                <v-col cols="12">
                  <v-expansion-panels>
                    <v-expansion-panel>
                      <v-expansion-panel-title class="bg-grey-darken-3">
                        PINOUT
                      </v-expansion-panel-title>
                      <v-expansion-panel-text>
                        <!--TODO: add highlight -->
                        <v-textarea v-model="formattedPinout" label="Pinout JSON" readonly auto-grow rows="4" />
                      </v-expansion-panel-text>
                    </v-expansion-panel>
                  </v-expansion-panels>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn color="blue-darken-1" variant="tonal" @click="close">
              Cancel
            </v-btn>
            <v-btn color="blue-darken-1" variant="tonal" :loading="loading" @click="saveAndContinue">
              Save and continue
            </v-btn>
            <v-btn color="blue-darken-1" variant="tonal" :loading="loading" @click="save">
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-toolbar>
  </template>
  <template #[`item.actions`]="{ item }">
    <v-icon size="small" class="me-2" @click="editItem(item)">
      mdi-pencil
    </v-icon>
    <v-icon size="small" @click="deleteItem(item)"> mdi-delete </v-icon>
  </template>
</v-data-table></template>

<script>
import { ref, reactive, computed, onMounted, watch } from "vue";
import { fa, tr } from "vuetify/locale";

export default {
  setup() {
    const apiUrl = "http://localhost:8081/api";
    const miniPcieModulesTypes = ref([]);
    const manufacturers = ref([]);
    const controllerTypes = ref([]);
    const m2ModuleTypes = ref([]);
    const controllersPcbHwVersions = ref([]);
    const projects = ref([]);
    const customers = ref([]);

    const pinoutID = ref(null);
    const typePinout = ref(null);
    const formattedPinout = ref("");
    const connectionTypes = ref("");
    const isFormDataLoaded = ref(false);
    const displayTypes = ref([]);

    const search = ref("");
    const selected = ref([]);
    const dialog = ref(false);
    const controllers = ref([]);
    const editedIndex = ref(-1);
    const loading = ref(true);
    const editedItem = reactive({
      customerId: null,
      typesId: null,
      projectsId: null,
      description: null,
      pcbHwVersionsId: null,
      serialNumber: null,
      manufacturersId: null,
      assemblyDate: null,
      macAddress: null,
      simNumber: null,
      articleNumber: null,
      infoQrCode: null,
      usb: false,
      serial: false,
      manufacturerQrCode: null,
      orderId: null,
      can1Terminated: false,
      can2Terminated: false,
      can3Terminated: false,
      can4Terminated: false,
      m2ModulesTypesId: null,
      displayTypeName: null,
      displayManufacturerQrCode: null,
      encloserSerialNumber: null,
      encloserManufacturerId: null,
      miniPcieTypeId: null,
      miniPcieSerialNumber: null,
      ledBoardQrCode: null,
      slotPinoutJson: {},
    });
    const defaultItem = { ...editedItem };

    const headers = [
      { title: "Controller Type", key: "typeName" },
      { title: "Controller PCB HW Version", key: "pcbHwVersion" },
      { title: "Controller Serial Number", key: "serialNumber" },
      { title: "Project", key: "projectName" },
      { title: "Description", key: "description" },
      { title: "Article Number", key: "articleNumber" },
      { title: "Display Adapter Type", key: "displayType" },
      { title: "Assembly Date", key: "assemblyDate" },
      { title: "Order ID", key: "orderID" },
      {
        title: "Controller Manufacturer QR",
        key: "manufacturerQrCodes",
      },
      { title: "Actions", key: "actions", sortable: false },
    ];

    const formTitle = computed(() =>
      editedIndex.value === -1 ? "New Controller" : "Edit Controller",
    );

    watch(connectionTypes, connectionTypesSelection);
    function connectionTypesSelection() {
      if (connectionTypes.value === "usb") {
        editedItem.usb = true;
        editedItem.serial = false;
      } else if (connectionTypes.value === "serial") {
        editedItem.usb = false;
        editedItem.serial = true;
      }
    }

    watch(() => editedItem.typesId, fetchControllersTypePinout);
    async function fetchControllersTypePinout() {
      try {
        pinoutID.value = editedItem.typesId;

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

    async function addNewController() {
      loading.value = true;
      try {
        const response = await fetch("http://localhost:8081/api/controllers", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(editedItem),
        });
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const newControllerData = await response.json();
        controllers.value.push(newControllerData);
      } catch (error) {
        console.error("Error adding new controller:", error);
      } finally {
        loading.value = false;
      }
    }

    async function fetchAllControllers() {
      loading.value = true;
      try {
        //TODO add global url
        const response = await fetch("http://localhost:8081/api/controllers");
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        controllers.value = data;
      } catch (error) {
        console.error("Error fetching controllers:", error);
      } finally {
        loading.value = false;
      }
    }

    async function fetchAll(endpoint, targetValue) {
      try {
        const response = await fetch(apiUrl + endpoint);
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        targetValue.value = data;
      } catch (error) {
        console.error("Error fetching " + endpoint + ": ", error);
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
      connectionTypes.value = "";
    }

    function save() {
      if (editedIndex.value > -1) {
        // Object.assign(controllers.value[editedIndex.value], editedItem);
      } else {
        // controllers.value.push({ ...editedItem, id: crypto.randomUUID() });
        addNewController();
      }
      close();
    }

    function saveAndContinue() {
      if (editedIndex.value > -1) {
        // Object.assign(controllers.value[editedIndex.value], editedItem);
      } else {
        // controllers.value.push({ ...editedItem, id: crypto.randomUUID() });
        addNewController();
      }
    }

    watch(dialog, async (newValue) => {
      if (newValue && !isFormDataLoaded.value) {
        loading.value = true;
        try {
          await Promise.all([
            fetchAll("/projects", projects),
            fetchAll("/customers", customers),
            fetchAll("/display/types", displayTypes),
            fetchAll("/controllers/types", controllerTypes),
            fetchAll("/miniPcieModules/types", miniPcieModulesTypes),
            fetchAll("/m2Modules/types", m2ModuleTypes),
            fetchAll("/controllers/pcbHwVersions", controllersPcbHwVersions),
            fetchAll("/manufacturers", manufacturers),
          ]);
          isFormDataLoaded.value = true;
        } catch (error) {
          console.error("Error loading form data:", error);
        } finally {
          loading.value = false;
        }
      }
    });

    onMounted(() => {
      fetchAllControllers();
    });

    return {
      apiUrl,
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
      connectionTypes,
      isFormDataLoaded,
      customers,
      projects,
      displayTypes,
    };
  },
};
</script>

<style scoped></style>

<template>
    <div class="min-h-screen p-6 card">
        <div class="flex justify-between items-center p-4 bg-white shadow-md rounded-md">
            <div class="flex items-center space-x-2">
                <Icon :icon="'mdi:trash-can'" class="w-6 h-6 text-gray-800 dark:text-gray-600" />
                <h4 class="text-lg font-medium text-gray-800 dark:text-black">Bin Management</h4>
            </div>
            <div class="flex items-center space-x-4">
                <span class="text-sm font-medium text-gray-600 dark:text-gray-400">Add Bin</span>
                <button @click="openAddBinModal"
                    class="w-8 h-8 flex items-center justify-center text-blue-500 hover:text-blue-700 rounded-full bg-blue-100 hover:bg-blue-200 focus:outline-none"
                    title="Add">
                    <Icon :icon="'mdi:add'" class="w-5 h-5" />
                </button>
            </div>
        </div>

        <ReusableTable :headers="['Location', 'Latitude', 'Longitude', 'Status', 'Actions']" :data="filteredBins"
            :fields="['locationName', 'latitude', 'longitude', 'status', 'actions']">
            <template #locationName="{ row }">
                <div class="text-base font-medium text-gray-900 dark:text-white">{{ row.locationName }}</div>
            </template>

            <template #latitude="{ row }">
                <div class="text-base text-gray-900 dark:text-white">{{ row.latitude }}</div>
            </template>

            <template #longitude="{ row }">
                <div class="text-base text-gray-900 dark:text-white">{{ row.longitude }}</div>
            </template>

            <template #status="{ row }">
                <div :class="{
                    'bg-green-100 text-green-700': row.status <=15,
                    'bg-yellow-100 text-yellow-700': row.status >15 && row.status <85,
                    'bg-red-100 text-red-700': row.status >85,
                }"
                    class="inline-flex items-center justify-center px-3 py-1 text-sm font-medium rounded-full shadow-sm">
                    {{ row.status }}
                </div>
            </template>

            <template #actions="{ row }">
                <div class="flex space-x-2">
                    <button @click="openEditBinModal(row)"
                        class="w-8 h-8 flex items-center justify-center text-blue-500 hover:text-blue-700 rounded-full bg-blue-100 hover:bg-blue-200 focus:outline-none"
                        title="Edit">
                        <Icon :icon="'mdi:pencil'" class="w-5 h-5" />
                    </button>
                    <button @click="deleteBin(row.id)"
                        class="w-8 h-8 flex items-center justify-center text-red-500 hover:text-red-700 rounded-full bg-red-100 hover:bg-red-200 focus:outline-none"
                        title="Delete">
                        <Icon :icon="'mdi:trash-can'" class="w-5 h-5" />
                    </button>
                </div>
            </template>
        </ReusableTable>

        <EditBinModal v-if="showEditBinModal" :initial-data="currentBin" @save="saveBinChanges"
            @close="closeEditBinModal" />
    </div>
</template>

<script>
import ReusableTable from "../components/ReusableTable/ReusableTable.vue";
import EditBinModal from "../components/BinsModal/BinsModal.vue";
import { Icon } from "@iconify/vue";
import { ref, computed, onMounted } from "vue";
import {
    fetchAllBinsApi,
    createBinApi,
    updateBinDataApi,
    deleteBinApi
} from '../api/bins';

export default {
    components: { ReusableTable, EditBinModal, Icon },
    setup() {
        const bins = ref([]);
        const showEditBinModal = ref(false);
        const currentBin = ref(null);

        const filteredBins = computed(() => bins.value);

        onMounted(async () => {
            await loadBins();
        });

        const loadBins = async () => {
            try {
                const fetchedBins = await fetchAllBinsApi();
                bins.value = fetchedBins.map(bin => ({
                    id: bin.id,
                    locationName: bin.locationName,
                    latitude: bin.latitude,
                    longitude: bin.longitude,
                    status: bin.status
                }));
            } catch (error) {
                console.error("Error fetching bins:", error);
            }
        };

        const openAddBinModal = () => {
            currentBin.value = null; // Clear data for adding a new bin
            showEditBinModal.value = true;
        };

        const openEditBinModal = (bin) => {
            currentBin.value = { ...bin }; // Ensure the bin object includes the id
            showEditBinModal.value = true;
        };

        const closeEditBinModal = () => {
            showEditBinModal.value = false;
        };

        const saveBinChanges = async (updatedData) => {
            if (currentBin.value) {
                // Update existing bin
                try {
                    await updateBinDataApi(currentBin.value.id, {
                        ...currentBin.value,
                        locationName: updatedData.locationName,
                        ...updatedData, // Merge existing data with updated data
                    });
                    await loadBins(); // Reload bins after update
                } catch (error) {
                    console.error("Error updating bin:", error);
                }
            } else {
                // Add new bin
                try {
                    await createBinApi({
                        ...updatedData, // Ensure updatedData contains all necessary fields
                        locationName: updatedData.locationName, // Map location to locationName if needed
                    });
                    await loadBins(); // Reload bins after creation
                } catch (error) {
                    console.error("Error creating bin:", error);
                }
            }
            closeEditBinModal();
        };

        const deleteBin = async (id) => {
            try {
                await deleteBinApi(id);
                await loadBins(); // Reload bins after deletion
            } catch (error) {
                console.error("Error deleting bin:", error);
            }
        };

        return {
            bins,
            filteredBins,
            showEditBinModal,
            currentBin,
            openAddBinModal,
            openEditBinModal,
            closeEditBinModal,
            saveBinChanges,
            deleteBin,
        };
    },
};
</script>

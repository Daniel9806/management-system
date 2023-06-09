<template>
    <div class="space-y-4 pr-6">
        <div class="flex space-x-2">
            <h1>Users</h1>
            <button @click="modalCreateActive = true"
                class="flex items-center px-4 bg-main-color rounded-t-lg rounded-bl-lg">
                <i class="material-icons">add</i>
            </button>
        </div>

        <LoadingSpinner class="mx-auto translate-y-20" v-if="userStore.getLoadingFetching" />

        <TableGeneric v-else :filteredList="userStore.getUsers" :header="header" :fields="fields" :greenAction="'Edit'"
            :blueAction="'Details'" :redAction="'Delete'" @onGreenAction="onEdit($event)" @onBlueAction="onDetails($event)"
            @onRedAction="onDelete($event)" />

        <Teleport to="#modal">
            <Transition name="modal">
                <modal-generic v-if="modalCreateActive" @closeModal="modalCreateActive = false" maxWidth="400"
                    title="New User">
                    <UserCreate @user-created="userCreated()" />
                </modal-generic>
            </Transition>
        </Teleport>

        <Teleport to="#modal">
            <Transition name="modal">
                <modal-generic v-if="modalEditActive" 
                @closeModal="modalEditActive = false" maxWidth="400"
                    title="Edit User">
                  <UserEdit :item-selected="itemSelected" @user-edited="refreshUsers()"/>
                </modal-generic>
            </Transition>
        </Teleport>

        <Teleport to="#modal">
            <Transition name="modal">
                <modal-generic v-if="modalDetailsActive" @closeModal="modalDetailsActive = false" maxWidth="600"
                    title="User Details">
                    <UserDetails :item="itemSelected" />
                </modal-generic>
            </Transition>
        </Teleport>
    </div>
</template>

<script setup>
import TableGeneric from '../components/tools/TableGeneric.vue'
import ModalGeneric from '../components/tools/ModalGeneric.vue'
import { reactive, ref, onMounted } from 'vue'
import { useUserStore } from '../store/userStore.js'
import LoadingSpinner from '../components/tools/LoadingSpinner.vue'
import UserCreate from '../components/modalViews/UserCreate.vue'
import UserDetails from '../components/modalViews/UserDetails.vue'
import UserEdit from '../components/modalViews/UserEdit.vue'
import { useAlert } from '../composables/useAlert'

const userStore = useUserStore()
const { confirmAlert, timerToast } = useAlert()

// const users = ref([])
const modalCreateActive = ref(false)
const modalDetailsActive = ref(false)
const modalEditActive = ref(false)
const itemSelected = ref({})

const fields = [
    'username', 'name', 'lastname', 'surname', 'confirmed'
]

const header = [
    'Username', 'Name', 'Last Name', 'Surname', 'Confirmed'
]

const onEdit = (item) => {
    modalEditActive.value = true
    itemSelected.value = item
}

const onDetails = (item) => {
    modalDetailsActive.value = true
    itemSelected.value = item
}

const onDelete = async (item) => {
    confirmAlert.fire({
        icon: "question",
        text: "Desea eliminar el usuario?",
    })
        .then(async (result) => {
            if (result.isConfirmed) {
                await userStore.deleteUser(item)
                await userStore.fetchUsers()
            }
        });
}

const refreshUsers = async () => {
    await userStore.fetchUsers()
}

const userCreated = async () => {
    await refreshUsers()
    modalCreateActive.value = false
}

onMounted(async () => {
    if (userStore.getUsers.length == 0) {
        await userStore.fetchUsers()
    }

})

</script>

<style scoped>
.col {
    padding: 10px;
}
</style>
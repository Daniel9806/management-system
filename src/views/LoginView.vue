<template>
    <div class="">
        <div class="p-4 mx-auto mt-28 max-w-sm bg-gray-300 rounded-lg border
                               border-gray-200 shadow-md sm:p-6 lg:p-8 dark:bg-gray-800 dark:border-gray-700">
            <form class="space-y-6">
                <h5 class="text-xl font-medium text-gray-900 dark:text-white">
                    Inicio de Sesión
                </h5>
                <div>
                    <label class="block  mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Nombre de
                        Usuario</label>
                    <div class="relative flex items-center">
                        <i class="material-icons absolute ml-3 text-lg text-black">person</i>
                        <input v-model="user.username" type="text"
                            class="bg-gray-50 pl-10 outline-none border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                            placeholder="username" />
                    </div>
                </div>
                <div>
                    <label class="block mb-2 text-sm font-medium text-gray-900 dark:text-gray-300">Contraseña</label>
                    <div class="relative flex items-center">
                        <i class="material-icons absolute ml-3 text-lg text-black">lock</i>
                        <input v-model="user.password" type="password" placeholder="••••••••"
                            class="bg-gray-50 pl-10 outline-none border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white" />
                    </div>
                </div>
                <div class="flex items-start">
                    <div class="flex items-start">
                        <div class="flex items-center h-5">
                            <input type="checkbox"
                                class="w-4 h-4 bg-gray-50 rounded border border-gray-300 focus:ring-3 focus:ring-blue-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-blue-600 dark:ring-offset-gray-800" />
                        </div>
                        <label class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Recordarme</label>
                    </div>
                    <a href="#" class="ml-auto text-sm text-main-color hover:underline">Olvidaste la
                        contraseña?</a>
                </div>
                <!-- <button @click.prevent="login" class="flex justify-center items-center space-x-2 w-full text-white
                 bg-main-color font-medium rounded-lg text-sm px-5
                  py-2.5 text-center outline-none">
                    <svg v-if="authStore.getLoadingAuth" role="status"
                        class="mt-[3px] w-4 h-4 text-gray-200 animate-spin dark:text-gray-900"
                        viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path
                            d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                            fill="currentColor" />
                        <path
                            d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                            fill="#1C64F2" />
                    </svg>
                    <p>Entrar</p>
                </button> -->
                <Button label="Login" @click.prevent="login" :loading="authStore.getLoadingAuth" />
            </form>
        </div>
    </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
import { reactive } from 'vue'
import { useAuthStore } from '../store/authStore.js';

const router = useRouter()
const authStore = useAuthStore()

const user = reactive({
    username: 'super',
    password: 'a'
})

const login = async () => {
    const resp = await authStore.login(user)
    if (resp) {
        router.push('/dash')
    }
}

// const login = () => {
//     router.push('/dash')
// }

</script>


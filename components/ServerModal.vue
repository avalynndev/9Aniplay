<template>
    <TransitionRoot appear :show="isOpen">
        <div>
            <TransitionChild as="template" enter="duration-300 ease-out" enter-from="opacity-0" enter-to="opacity-100"
                leave="duration-200 ease-in" leave-from="opacity-100" leave-to="opacity-0">
                <div class="fixed inset-0 bg-black bg-opacity-50" @click="toggleModal" />
            </TransitionChild>

            <div class="fixed inset-0 overflow-y-auto z-50">
                <div class="flex min-h-full items-center justify-center p-4 text-center">
                    <TransitionChild as="template" enter="duration-300 ease-out" enter-from="opacity-0 scale-95"
                        enter-to="opacity-100 scale-100" leave="duration-200 ease-in" leave-from="opacity-100 scale-100"
                        leave-to="opacity-0 scale-95">
                        <div
                            class="w-full max-w-md transform overflow-hidden rounded-2xl bg-neutral-800 p-6 text-left align-middle shadow-xl transition-all">
                            <div as="h3" class="text-lg font-medium leading-6 text-white mb-5">
                                Please select a server
                            </div>
                            <div class="mt-2">
                                <div class="active:bg-violet-500 hover:bg-violet-500 text-white px-4 py-2 rounded-full mb-3"
                                    v-for="server in serverList" :key="server"
                                    :class="activeServer == server.id ? 'bg-violet-500' : 'bg-purple-800'">
                                    <label class="block ml-4" :for="server.id" @click="setServer(server.id)">
                                        {{ server.name }}
                                    </label>
                                </div>
                            </div>

                            <div class="mt-5">
                                <button type="button"
                                    class="inline-flex justify-center rounded-md border border-transparent bg-blue-100 px-4 py-2 text-sm font-medium text-blue-900 hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2"
                                    @click="closeModal">
                                    Confirm!
                                </button>
                            </div>
                        </div>
                    </TransitionChild>
                </div>
            </div>
        </div>
    </TransitionRoot>
</template>

<script setup>
import { ref } from 'vue'
import {
    TransitionRoot,
    TransitionChild
} from '@headlessui/vue'

const props = defineProps({
    isOpenModal: Boolean
})

var isOpen = ref(props.isOpenModal)

const serverList = [
    { id: 'gogoanime', name: 'Gogoanime (Preferred)' },
    { id: 'zoro', name: 'Zoro.to' },
]

var activeServer = localStorage.getItem('server') == 'gogoanime' ? 'gogoanime' : 'zoro'

function closeModal() {
    isOpen.value = false
    window, location.reload()
}
function openModal() {
    isOpen.value = true
}

function toggleModal() {
    isOpen.value = !isOpen.value
}

function setServer(server) {
    // console.log('setServer', server)
    activeServer = server
    localStorage.setItem('server', server)
}
</script>
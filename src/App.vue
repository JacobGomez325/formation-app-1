<template>
  <div>
    <vmodal v-if="showModal" :title="tools.title" @close="close">
      <div class="w-[500px] h-[200px] overflow-auto">
        <form  @submit.prevent="sendForm" class="p-4">
          <div class="grid grid-cols-2 gap-4">
            <input 
            v-model="formData.name"
            type="text" 
            required
            placeholder="Entrez le name"
            class="border border-gray-500 px-4 py-2 outline-none "
            >
            <input 
            v-model="formData.username"
            type="text" 
            required
            placeholder="Entrez le username"
            class="border border-gray-500 px-4 py-2 outline-none "
            >
          </div>
          <div class="grid grid-cols-2 gap-4 my-2">
            <input 
            v-model="formData.email"
            type="email"
            required
            placeholder="Entrez l'email"
            class="border border-gray-500 px-4 py-2 outline-none "
            >
            <input 
            v-model="formData.phone"
            type="text"
            required
            placeholder="Entrez le numero de telephone"
            pattern="^\+229\d{8}$"
            class="border border-gray-500 px-4 py-2 outline-none "
            >
          </div>
          <div class="flex justify-center space-x-4 my-4">
            <mon-button 
              styles="bg-blue-500 text-white px-4 py-2 shadow outline-none " 
              types="submit">
              <span v-if="!tools.isUpdate"> Ajouter</span>
              <span v-else> Modifier</span>
            </mon-button>
            <mon-button 
              styles="bg-red-500 text-white px-4 py-2 shadow outline-none " 
              types="reset">
             Annuler
            </mon-button>
            
           
            
          
          </div>
        </form>
      </div>
    </vmodal>
    <h1 class="text-2xl uppercase font-semibold">Application 1</h1>

    <div class=" container mx-auto my-4 flex justify-between">
      <input type="text" v-model="tools.search" class="border border-gray-700 p-3 outline-none" placeholder="recherche...">
      <button @click="openModal" class="bg-blue-500 p-3 text-white ">Ajouter un membre</button>
    </div>
    
   
    <UserTable :datas="resultQuery" @showData="showData" /> 

  </div>
</template>

<script setup>
import {ref,reactive,computed} from 'vue'
import MonButton from './components/MonButton.vue'
import UserTable from './components/UserTable.vue'
import Vmodal from '@/components/Vmodal.vue'
import { Users } from '../data/user'
const showModal = ref(false)
const demoS = ' <p alert="ddd"> demo </p>'
const UsersData = ref(Users) 
const tools = reactive({
  title:'Ajouter un membre',
  isUpdate:false,
  index:0,
  search:''
})
const formData  = reactive(
  {
    "id":0,
    "name": "",
    "username": "",
    "email": "",
    "phone": "",
    "website": "https://www.youtube.com/@gojanda"
  }
)


function close() {
  showModal.value = false
  vider()
}

function vider () {
  formData.id = 0
  formData.name = ''
  formData.username = ''
  formData.email =''
  formData.phone =''
}
function sendForm() {
  if(!tools.isUpdate) {
    const id = Math.floor(Math.random() * 1000)
    formData.id = id
    const copyformData = {...formData}
    UsersData.value.push(copyformData)
    vider()
  }else {
    UsersData.value[tools.index] = {...formData}
    close()
  }
 
}
function showData(index,data) {
  tools.index = index
  tools.isUpdate = true
  showModal.value = true
  tools.title = "Modifier un membre"
  formData.name = data.name
  formData.username = data.username
  formData.email = data.email
  formData.phone =data.phone
}
function openModal() {
  showModal.value = true
  tools.isUpdate = false
  tools.title = "Ajouter un membre"
}

const resultQuery = computed(() => {
  if(tools.search){
    return UsersData.value.filter((item)=>{
      return tools.search.toLowerCase().split(' ').every(v => item.name.toLowerCase().includes(v)) ||
      tools.search.toLowerCase().split(' ').every(v => item.username.toLowerCase().includes(v)) ||
      tools.search.toLowerCase().split(' ').every(v => item.email.toLowerCase().includes(v)) ||
      tools.search.toLowerCase().split(' ').every(v => item.phone.toLowerCase().includes(v)) 
    })
  }else{
    return UsersData.value;
  }
    
})


</script>

<style lang="scss" scoped>

</style>
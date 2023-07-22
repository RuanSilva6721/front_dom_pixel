<template>
  
  <div class="q-pa-md row items-start q-gutter-md">
    <q-card v-for="(item, index) in items" :key="index" class="my-card" flat bordered>
      <q-img src="https://cdn.quasar.dev/img/parallax2.jpg" />

      <q-card-section>
        <div class="text-h5 q-mt-sm q-mb-xs">{{ item.name }}</div>
        <div class="text-caption text-grey">{{ item.voltage }}</div>
      </q-card-section>

      <q-card-actions>
        <q-btn flat color="primary" label="Editar" @click="openModal(item)" />
        <q-btn flat color="red" label="Deletar" @click="deleteItem(item.id)" />

        <q-space />

        <q-btn
          color="grey"
          round
          flat
          dense
          :icon="item.expanded ? 'keyboard_arrow_up' : 'keyboard_arrow_down'"
          @click="toggleExpand(item)"
        />
      </q-card-actions>

      <q-slide-transition>
        <div v-show="item.expanded">
          <q-separator />
          <q-card-section class="text-subtitle2">{{ item.description }}</q-card-section>
        </div>
      </q-slide-transition>
    </q-card>

    <!-- Modal para edição -->
    <q-dialog v-model="showModal">
      <q-card>
        <q-card-section>
          <div class="text-h6">Editar Item</div>

          <!-- Formulário de edição -->
          <q-input v-model="editedItem.name" label="Nome" filled />
          <q-input v-model="editedItem.voltage" label="Voltagem" filled />
          <q-input v-model="editedItem.description" label="Descrição" filled type="textarea" />
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat color="primary" label="Salvar" @click="saveItem" />
          <q-btn flat color="red" label="Cancelar" @click="closeModal" />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <!-- Modal para exibir mensagem de sucesso ou erro -->
    <q-dialog v-model="showMessageModal">
      <q-card>
        <q-card-section>
          <div class="text-h6">{{ messageTitle }}</div>
          <div class="text-subtitle2">{{ message }}</div>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat color="primary" label="OK" @click="closeMessageModal" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script setup lang="ts">
import { ref, onBeforeMount } from 'vue'
import api from "../axios/api";

const props = defineProps({
  productData: Array
});

const items = ref();
const showModal = ref(false);
const selectedItem = ref(null);
const editedItem = ref();
const showMessageModal = ref(false);
const messageTitle = ref('');
const message = ref('');

onBeforeMount(() => {
  if (props.productData) {
    items.value = props.productData;
  }
});

function toggleExpand(item) {
  item.expanded = !item.expanded;
}

function openModal(item) {
  selectedItem.value = item;
  editedItem.value = { ...item }; // Copia os valores do item selecionado 
  showModal.value = true;
}

function closeModal() {
  showModal.value = false;
}

async function saveItem() {
  try {
    const responseData = await api.put(`/api/applianceProduct/${editedItem.value.id}`, {
      description: editedItem.value.description,
      name: editedItem.value.name,
      voltage: editedItem.value.voltage
    });
    messageTitle.value = 'Sucesso';
    message.value = 'Item salvo com sucesso.';
  } catch (err) {
    console.error(err);
    messageTitle.value = 'Erro';
    message.value = 'Ocorreu um erro ao salvar o item.';
  }
  showMessageModal.value = true;
  closeModal();
}

async function deleteItem(itemId) {
  try {
    const responseData = await api.delete(`/api/applianceProduct/${itemId}`);

    messageTitle.value = 'Sucesso';
    message.value = 'Item excluído com sucesso.';
  } catch (err) {
    console.error(err);
    messageTitle.value = 'Erro';
    message.value = 'Ocorreu um erro ao excluir o item.';
  }
  showMessageModal.value = true;
  closeModal();
}

function closeMessageModal() {
  showMessageModal.value = false;
  window.location.reload(); 
}
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 350px


</style>

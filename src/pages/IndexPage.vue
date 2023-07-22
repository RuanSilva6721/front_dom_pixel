<template>
 
    <TitleIndexComponent />
    <MenuAdminComponent/>
  <div > 
    <h5 class="title">Catálogo de Produtos</h5>
      <CardComponent :productData="productData" />
  </div>
  <div>
    Não há Produtos cadastrados!!
  </div>
</template>


<script setup lang="ts">
import { Todo, Meta } from 'components/models';
import CardComponent from 'src/components/CardComponent.vue';
import TitleIndexComponent from 'src/components/TitleIndexComponent.vue';
import { ref, onMounted } from 'vue';
import api from "../axios/api";
import MenuAdminComponent from 'src/components/MenuAdminComponent.vue';

const productData = ref([]);
async function getProductAllData() {
  try {
    const responseData = await api.get('/api/applianceProduct');
    productData.value = responseData.data;
    console.log(productData.value);
  } catch (err) {
    console.error(err);
  }
}

onMounted(async () => {
  await getProductAllData();
});

const meta = ref<Meta>({
  totalCount: 1200,
});
</script>
<style lang="sass" scoped>

.title
  font-size: 1.2rem
  font-weight: bold
  color: #333
  text-transform: uppercase
  margin-bottom: 0
  margin-left: 70px
</style>
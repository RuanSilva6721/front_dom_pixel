<template>
  <div class="q-pa-md">
    <h5 class="brand-title">Criar Marca ou Produto</h5>
    <div class="q-pa-md row items-start q-gutter-md">       
    <q-card class="my-card">
      <a
        href="/create-product"
        class="custom-link"
      >
      <q-card-section class="bg-primary text-white">
        <div class="text-h6">Criar Produto</div>
      </q-card-section>
    </a>
    </q-card>
    </div>
  </div>
</template>

<script setup lang="ts">
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 350px

.brand-title 
  font-size: 1.2rem
  font-weight: bold
  color: #333
  text-transform: uppercase
  margin-bottom: 0
  margin-left: 60px

.admin-menu
  display: flex
  flex-wrap: wrap
  justify-content: center
  gap: 16px
  margin-bottom: 60px
  margin-top: 60px

.admin-menu-item
  display: flex
  flex-direction: column
  align-items: center
  cursor: pointer

  .q-icon
    margin-bottom: 8px

.custom-link 
  color: inherit
  text-decoration: none
  cursor: pointer
</style>

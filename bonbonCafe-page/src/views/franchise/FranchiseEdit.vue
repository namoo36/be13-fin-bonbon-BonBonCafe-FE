<template>
  <v-container class="containerF">
    <v-row justify="center" class="pa-10 pb-0">
          <v-col>
            <v-row class="mb-6">
              <v-icon color="primary" size="40">mdi-store-plus</v-icon>
              <div class="text-h5 font-weight-bold mt-2">가맹점 관리 / 가맹점 정보 수정</div>
            </v-row>
            <v-divider class="my-4"></v-divider>
          </v-col>
          
        </v-row>
    <FranchiseForm
      :initialFormData="formData"
      :readonly="false"
      :submitVisible="true"
      mode="edit"
      @submit="handleSubmit"
      @back="router.back()"
    />
  </v-container>
</template>


<script setup>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import apiClient from '@/api'
import FranchiseForm from '@/components/franchise/FranchiseForm.vue'

const route = useRoute()
const router = useRouter()

const formData = ref(null)

const franchiseId = route.params.franchiseId


const fetchFranchiseDetail = async (franchiseId) => {
  try {
    const response = await apiClient.get(`/franchise/${franchiseId}`)
    formData.value = response.data
  } catch (error) {
    console.error('가맹점 정보 불러오기 실패:', error)
  }
}

onMounted(() => {
  fetchFranchiseDetail(franchiseId)
})

const handleSubmit = async (updatedData) => {
  console.log(updatedData, "===============");
  console.log(franchiseId, "===============");
  try {
    await apiClient.patch(`/franchise/${franchiseId}`, updatedData)
    alert('수정이 완료되었습니다.')
    router.push(`/franchise/${franchiseId}`)
  } catch (error) {
    console.error('수정 실패:', error)
    alert('수정 중 오류가 발생했습니다.')
  }
}
</script>


<style scoped>

</style>
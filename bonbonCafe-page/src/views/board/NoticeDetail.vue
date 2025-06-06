<template>
  <div class="notice-detail-wrapper ma-10 pa-8">
    <h2 class="text-2xl font-bold mb-4">{{ notice.title }}</h2>

    <div class="mb-4 text-grey-darken-1">
      <span>작성자: {{ notice.author }}</span> |
      <span>작성일: {{ formatDate(notice.createTime) }}</span>
    </div>

    <v-card class="pa-6 mb-4 elevation-1 content-card">
      <div v-html="notice.content" />
    </v-card>

    <div v-if="notice.sent" class="mb-4">
      <v-chip color="success" text-color="white">✅ 문자 전송 완료</v-chip>
    </div>

    <div class="d-flex justify-end" style="gap: 10px;">
      <v-btn :color="notice.sent ? 'grey' : 'secondary'" @click="sendSms">문자 일괄 전송</v-btn>
      <v-btn color="warning" @click="goToEdit">수정</v-btn>
      <v-btn color="error" @click="deleteNotice">삭제</v-btn>
      <v-btn color="primary" @click="goBack">목록으로</v-btn>
    </div>
  </div>
</template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import { useRoute, useRouter } from 'vue-router'
  import apiClient from '@/api'
  
  const route = useRoute()
  const router = useRouter()
  
  const headquarterId = route.params.headquarterId
  const noticeId = route.params.noticeId
  
  const notice = ref({})
  
  const fetchNotice = async () => {
  try {
    const { data } = await apiClient.get(`/notice/${noticeId}`)
    // 줄바꿈 처리
    data.content = data.content?.replace(/\n/g, '<br/>')
    notice.value = data
  } catch (e) {
    console.error('❌ 상세 조회 실패', e)
  }
}
  
  const sendSms = async () => {
    if (notice.value.sent) {
      const confirmed = confirm('이미 문자 보낸 적이 있습니다.\n다시 보내시겠습니까?')
      if (!confirmed) return
    }
  
    try {
      await apiClient.post(`/notice/${noticeId}/send-sms`)
      alert('문자 일괄 전송 완료!')
      fetchNotice()
    } catch (e) {
      console.error('❌ 문자 전송 실패', e)
      alert('문자 전송 중 오류가 발생했습니다.')
    }
  }
  
  const goToEdit = () => {
    router.push(`/notices/${noticeId}/edit`)
  }
  
  const deleteNotice = async () => {
    const confirmed = confirm('정말 이 공지사항을 삭제하시겠습니까?')
    if (!confirmed) return
  
    try {
      await apiClient.delete(`/notice/${noticeId}`)
      alert('삭제되었습니다.')
      router.push('/notice-list') // 그냥 문자열로 이동
    } catch (e) {
      console.error('❌ 삭제 실패', e)
      alert('삭제 중 오류가 발생했습니다.')
    }
  }
  
  const goBack = () => {
    router.push('/notice-list')
}
  
  const formatDate = (dateTime) => {
    return new Date(dateTime).toLocaleDateString()
  }
  
  onMounted(fetchNotice)
  </script>
  
  <style scoped>
.notice-detail-wrapper {
  background-color: #f9f9f9;
  border-radius: 12px;
}

.content-card {
  border-radius: 12px;
  background-color: #ffffff;
  line-height: 1.6;
  white-space: normal;
}
  </style>
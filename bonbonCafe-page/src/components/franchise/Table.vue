<template>
    <v-table class=" rounded-t-xl">
        <thead class="theadColor">
            <tr>
                <th id="tt">NO.</th>
                <th>가맹점 이름</th>
                <th>가맹점 주소</th>
                <th>담당자 </th>
                <th>가맹점 연락처</th>
                <th>등록일</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in item" :key="item.name" @click="goToDetail(item)" style="cursor: pointer;">
                <td>{{ item.franchiseId }}</td>
                <td>{{ item.name }}</td>
                <td>{{ item.roadAddress }}</td>
                <td>{{ item.managerName }}</td>
                <td>{{ item.franchiseTel }}</td>
                <td>{{ item.openDate }}</td>
            </tr>
        </tbody>
    </v-table>
</template>

<script setup>
    import { useRouter } from 'vue-router'

    const props = defineProps({
        item:{
            type: Array,
            required: true
        },
    });
    const router = useRouter()

    const goToDetail = (item) => {
    router.push({
        name: 'franchise-detail',
        params: { franchiseId: item.franchiseId }
    });
    };

</script>

<style scoped>
.theadColor{
    background-color: #D8DBBD;
}
</style>
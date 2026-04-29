<template>
  <div id="spaceDetailPage">
    <!-- 空间信息 -->
    <a-flex justify="space-between">
      <h2>{{ space.spaceName }}（私有空间）</h2>
      <a-space size="middle">
        <a-button type="primary" :href="`/add_picture?spaceId=${id}`" target="_blank">
          + 创建图片
        </a-button>
        <a-button :href="`/add_space?id=${id}`" target="_blank">
          编辑空间
        </a-button>
        <a-tooltip
          :title="`占用空间 ${formatSize(space.totalSize)} / ${formatSize(space.maxSize)}`"
        >
          <a-progress
            type="circle"
            :percent="Number(((space.totalSize ?? 0) * 100 / (space.maxSize ?? 1)).toFixed(1))"
            :size="42"
          />
        </a-tooltip>
      </a-space>
    </a-flex>
    <!-- 图片列表 -->
    <PictureList :dataList="dataList" :loading="loading" showOp :onReload="fetchData" />
    <a-pagination
      style="text-align: right"
      v-model:current="searchParams.current"
      v-model:pageSize="searchParams.pageSize"
      :total="total"
      :show-total="() => `图片总数 ${total} / ${space.maxCount ?? '∞'}`"
      @change="onPageChange"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, onMounted } from 'vue'
import { message } from 'ant-design-vue'
import PictureList from '@/components/PictureList.vue'
import { getSpaceVoByIdUsingGet } from '@/api/spaceController'
import { listPictureVoByPageUsingPost } from '@/api/pictureController'
import { formatSize } from '@/utils'

const props = defineProps<{
  id: string | number
}>()

const space = ref<API.SpaceVO>({})
const dataList = ref([])
const total = ref(0)
const loading = ref(true)

const searchParams = reactive<API.PictureQueryRequest>({
  current: 1,
  pageSize: 12,
  sortField: 'createTime',
  sortOrder: 'descend',
})

const onPageChange = (page: number, pageSize: number) => {
  searchParams.current = page
  searchParams.pageSize = pageSize
  fetchData()
}

const fetchSpaceDetail = async () => {
  try {
    const res = await getSpaceVoByIdUsingGet({
      id: props.id as any,
    })
    if (res.data.code === 0 && res.data.data) {
      space.value = res.data.data
    } else {
      message.error('获取空间详情失败，' + res.data.message)
    }
  } catch (e: any) {
    message.error('获取空间详情失败：' + e.message)
  }
}

const fetchData = async () => {
  loading.value = true
  const params: any = {
    spaceId: props.id,
    ...searchParams,
  }
  const res = await listPictureVoByPageUsingPost(params)
  if (res.data.code === 0 && res.data.data) {
    dataList.value = res.data.data.records ?? []
    total.value = res.data.data.total ?? 0
  } else {
    message.error('获取数据失败，' + res.data.message)
  }
  loading.value = false
}

onMounted(() => {
  fetchSpaceDetail()
  fetchData()
})
</script>

<style scoped>
#spaceDetailPage {
}
</style>

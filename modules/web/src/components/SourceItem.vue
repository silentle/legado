<template>
  <el-checkbox
    size="large"
    border
    :label="sourceUrl"
    :class="{
      error: isSaveError,
      edit: sourceUrl == currentSourceUrl,
    }"
  >
    {{ source.bookSourceName || source.sourceName }}
    <el-button text :icon="Edit" @click="handleSourceClick(source)" />
  </el-checkbox>
</template>

<script setup>
import { Edit } from "@element-plus/icons-vue";

const props = defineProps(["source"]);
const store = useSourceStore();
const { savedSourcesMap, currentSourceUrl, sourceUrlKey } = storeToRefs(store);
const sourceUrl = computed(() => props.source[sourceUrlKey.value]);
const handleSourceClick = (source) => {
  store.changeCurrentSource(source);
};
const isSaveError = computed(() => {
  if (savedSourcesMap.value.size == 0) return false;
  return !savedSourcesMap.value.has(sourceUrl.value);
});
</script>
<style lang="scss" scoped>
:deep(.el-checkbox__label) {
  flex: 1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.error {
  border-color: var(--el-color-error) !important;
  color: var(--el-color-error) !important;
  --el-checkbox-checked-text-color: var(--el-color-error);
  --el-checkbox-checked-bg-color: var(--el-color-error);
  --el-checkbox-checked-input-border-color: var(--el-color-error);
}
.edit {
  border-color: var(--el-color-dark) !important;
}
</style>

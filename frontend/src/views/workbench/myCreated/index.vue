<template>
  <div class="flex flex-col gap-[16px]">
    <div class="flex items-center justify-end gap-[12px]">
      <MsProjectSelect v-model:project="currentProject" class="w-[240px]" use-default-arrow-icon>
        <template #prefix>
          {{ t('menu.projectManagementShort') }}
        </template>
      </MsProjectSelect>
      <a-select
        v-model:model-value="features"
        :options="featureOptions"
        :max-tag-count="1"
        multiple
        class="w-[240px]"
        @change="handleFeatureChange"
      >
        <template #prefix>
          {{ t('project.messageManagement.function') }}
        </template>
        <template #header>
          <a-checkbox v-model:model-value="featureAll" class="ml-[8px]" @change="handleFeatureAllChange">
            {{ t('common.all') }}
          </a-checkbox>
        </template>
      </a-select>
      <a-button type="outline" class="arco-btn-outline--secondary p-[10px]" @click="handleRefresh">
        <MsIcon type="icon-icon_reset_outlined" size="14" />
      </a-button>
    </div>
    <testPlanTable v-if="features.includes(FeatureEnum.TEST_PLAN)" :project="currentProject" type="my_create" />
    <testCaseTable v-if="features.includes(FeatureEnum.TEST_CASE)" :project="currentProject" type="my_create" />
    <caseReviewTable v-if="features.includes(FeatureEnum.CASE_REVIEW)" :project="currentProject" type="my_create" />
    <apiCaseTable v-if="features.includes(FeatureEnum.API_CASE)" :project="currentProject" type="my_create" />
    <scenarioCaseTable v-if="features.includes(FeatureEnum.API_SCENARIO)" :project="currentProject" type="my_create" />
    <bugTable v-if="features.includes(FeatureEnum.BUG)" :project="currentProject" type="my_create" />
  </div>
</template>

<script setup lang="ts">
  import MsIcon from '@/components/pure/ms-icon-font/index.vue';
  import MsProjectSelect from '@/components/business/ms-project-select/index.vue';
  import apiCaseTable from '../components/apiCaseTable.vue';
  import bugTable from '../components/bugTable.vue';
  import caseReviewTable from '../components/caseReviewTable.vue';
  import scenarioCaseTable from '../components/scenarioCaseTable.vue';
  import testCaseTable from '../components/testCaseTable.vue';
  import testPlanTable from '../components/testPlanTable.vue';

  import { useI18n } from '@/hooks/useI18n';
  import useAppStore from '@/store/modules/app';

  import { FeatureEnum } from '@/enums/workbenchEnum';

  const { t } = useI18n();
  const appStore = useAppStore();

  const currentProject = ref(appStore.currentProjectId);
  const features = ref<FeatureEnum[]>(Object.values(FeatureEnum));
  const featureOptions = Object.keys(FeatureEnum).map((key) => ({
    label: t(`ms.workbench.myFollowed.feature.${key}`),
    value: key as FeatureEnum,
  }));
  const featureAll = ref(true);

  function handleFeatureAllChange(val: boolean | (string | number | boolean)[]) {
    features.value = val ? featureOptions.map((item) => item.value) : [];
  }

  function handleFeatureChange(
    val: string | number | boolean | Record<string, any> | (string | number | boolean | Record<string, any>)[]
  ) {
    featureAll.value = (val as []).length === featureOptions.length;
  }

  function handleRefresh() {
    console.log('refresh');
  }
</script>

<style lang="less" scoped></style>

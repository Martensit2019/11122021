pages\my-calculations\_id\index.vue

<template>
  <div>
    myCalculations - {{ myCalculations }}
    <div class="top">
      <h1 class="text-uppercase">{{ $t("view") }}</h1>
    </div>

    <div v-if="myCalculations">
        <div class="title">Расчет</div>        
      <view-field label="Номер расчета:" :value="myCalculations.id" />
      <view-field label="Регион объекта:" :value="myCalculations.region_id" />
      <view-field label="Дата создания:" :value="myCalculations.updated_at" />
      <view-field label="Тип расчета:" :value="myCalculations.name" />      
      <div class="title">Результаты расчета объема ёмкости</div>
      <view-field style="width: 410px" label="Месторасположение площади водосбора:" value="Ростов-на-Дону" />
      <view-field label="Регион, в котором расположена площадь водосбора:" value="Ростовская область" />
      <view-field label="Средний максимум суточного слоя осадков, Hср:" value="41.00" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />
      <view-field label="" value="" />

      <div class="group-btns">
        <nuxt-link :to="`${$route.params.id}/edit`" class="btn">{{
          $t("edit")
        }}</nuxt-link>
        <button class="btn" @click="remove()">{{ $t("delete") }}</button>
        <nuxt-link :to="{ name: 'admin-macroregions' }" class="btn">{{
          $t("back")
        }}</nuxt-link>
      </div>
    </div>

    <VueSpinner v-else />
  </div>
</template>

<script>
import ViewField from "@/components/forms/includes/ViewField";
import { loadMyCalculation } from "@/src/queries";
import VueSpinner from "@/components/vueSpinner";
export default {
  // middleware: "admin",
  name: "myCalculationsView",
  components: {
    VueSpinner,
    ViewField,
  },
  head() {
    return {
      title: this.$t("view"),
    };
  },
  data() {
    return {
      myCalculations: null,
    };
  },
  mounted() {
    const myCalculationsId = this.$route.params.id;
    console.log("myCalculationsId ->", myCalculationsId);

    loadMyCalculation(myCalculationsId)
      .then(({ data }) => {
        this.myCalculations = data;
        this.$helpers.setBreadcrumbsParams({
          id: this.myCalculations.name,
        });
      })
      .finally(() => console.log(this.macroregion));
  },
};
</script>

<style lang="scss" scoped>
.title{
font-weight: 500;
font-size: 12px;
line-height: 14px;
color: #006843;
margin-bottom: 30px;

}
</style>
------------------
export function loadMyCalculation(id) {
  return new Promise(function(resolve, reject) {
    window.$nuxt.$axios
      .get(`/api/calculations/${id}`)
      .then(({ data }) => resolve(data))
      .catch(error => reject(error));
  });
}

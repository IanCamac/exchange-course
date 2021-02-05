<template>
  <div>
    <bounce-loader :loading="isLoading" :color="'#68d391'" :size="100" />
    <px-assets-table v-if="!isLoading" :assets="assets" />
  </div>
</template>

<script>
import api from '@/api'
import PxAssetsTable from "@/components/PxAssetsTable";

export default {
  name: "Home",
  components: { PxAssetsTable },

  data() {
    return {
      isLoading: false,
      assets: [],
      interval: null
    }
  },

  async created () {
    this.isLoading = true;
    this.assets = await api.getAssets()
    this.isLoading = false
    this.refreshFetch();
  },

  methods: {
    refreshFetch() {
      this.interval = setInterval(async () => {
        this.assets = await api.getAssets();

      }, 5000);
    }
  },

  beforeDestroy() {
    clearInterval(this.interval);
  }
};
</script>

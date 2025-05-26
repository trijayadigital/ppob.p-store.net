<template>
  <div class="h-screen flex flex-col text-gray-700 font-display bg-slate-100 gap-2">
    <!-- Header -->
    <header class="flex-shrink-0 bg-white shadow p-8">
      <div class="max-w-6xl mx-auto flex items-center justify-between">
        <div class="flex items-center divide-x divide-gray-300 text-gray-500">
          <NuxtLink to="/">
            <img src="/logo.png" alt="Shopee" class="h-8 w-auto pr-6" />
          </NuxtLink>
          <span class="text-lg font-bold pl-6">Pulsa, Tagihan &amp; Hiburan</span>
        </div>
        <Button variant="outline" class="text-base cursor-pointer font-semibold">
          <Icon name="lucide:clipboard-list" size="21" />
          <span>Pesanan Saya</span>
        </Button>
      </div>
    </header>

    <!-- Navigation Tabs -->
    <div class="flex-shrink-0 max-w-6xl mx-auto w-full flex items-center justify-center bg-white rounded-lg shadow-sm p-2">
      <div class="">
        <div class="flex items-center space-x-1">
          <NuxtLink
            to="/pulsa"
            class="flex flex-col items-center px-4 py-3 rounded-lg transition-all duration-200 hover:bg-gray-50"
            :class="$route.path === '/pulsa' ? 'text-primary bg-primary/10' : 'text-gray-600'"
          >
            <Icon name="lucide:smartphone" size="24" />
            <span class="text-sm font-medium mt-1">Pulsa</span>
          </NuxtLink>

          <NuxtLink
            to="/paket-data"
            class="flex flex-col items-center px-4 py-3 rounded-lg transition-all duration-200 hover:bg-gray-50"
            :class="$route.path === '/paket-data' ? 'text-primary bg-primary/10' : 'text-gray-600'"
          >
            <Icon name="lucide:wifi" size="24" />
            <span class="text-sm font-medium mt-1">Paket Data</span>
          </NuxtLink>

          <NuxtLink
            to="/voucher-game"
            class="flex flex-col items-center px-4 py-3 rounded-lg transition-all duration-200 hover:bg-gray-50"
            :class="$route.path.startsWith('/voucher-game') ? 'text-primary bg-primary/10' : 'text-gray-600'"
          >
            <Icon name="lucide:gamepad" size="24" />
            <span class="text-sm font-medium mt-1">Voucher Game</span>
          </NuxtLink>

          <NuxtLink
            to="/tagihan"
            class="flex flex-col items-center px-4 py-3 rounded-lg transition-all duration-200 hover:bg-gray-50"
            :class="$route.path === '/tagihan' ? 'text-primary bg-primary/10' : 'text-gray-600'"
          >
            <Icon name="heroicons:bolt-solid" size="24" />
            <span class="text-sm font-medium mt-1">Tagihan</span>
          </NuxtLink>

          <NuxtLink
            to="/entertainment"
            class="flex flex-col items-center px-4 py-3 rounded-lg transition-all duration-200 hover:bg-gray-50"
            :class="$route.path === '/entertainment' ? 'text-primary bg-primary/10' : 'text-gray-600'"
          >
            <Icon name="lucide:music" size="24" />
            <span class="text-sm font-medium mt-1">Entertainment</span>
          </NuxtLink>

          <NuxtLink
            to="/lainnya"
            class="flex flex-col items-center px-4 py-3 rounded-lg transition-all duration-200 hover:bg-gray-50"
            :class="$route.path === '/lainnya' ? 'text-primary bg-primary/10' : 'text-gray-600'"
          >
            <Icon name="lucide:more-horizontal" size="24" />
            <span class="text-sm font-medium mt-1">Lainnya</span>
          </NuxtLink>
        </div>
      </div>
    </div>

    <!-- Main Content -->
    <div class="flex-1 overflow-hidden max-w-6xl mx-auto h-full">
      <slot />
    </div>

    <!-- Footer -->
    <div class="flex-shrink-0 bg-white border-t-2 border-gray-200 shadow-xl">
      <div class="max-w-6xl mx-auto px-4 py-4 flex gap-x-4 justify-end items-center">
        <div class="flex flex-col gap-y-1">
          <span class="text-xs text-gray-600">Total</span>
          <span class="text-primary text-base font-bold">{{ formattedPrice }}</span>
        </div>
        <Button class="bg-primary text-white px-8 py-6 rounded-lg hover:bg-primary/80 transition-colors" :disabled="!selectedProduct">
          Beli Sekarang
        </Button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { Button } from '@/components/ui/button';
import { ScrollArea } from '@/components/ui/scroll-area';

const selectedProduct = ref(null);

const formattedPrice = computed(() => {
  if (!selectedProduct.value || !selectedProduct.value.price_personal) return 'Rp 0';
  return `Rp ${Number(selectedProduct.value.price_personal).toLocaleString('id-ID')}`;
});

// Provide selectedProduct to child components
provide('selectedProduct', selectedProduct);
</script>

<style scoped>
.fixed {
  position: fixed;
  z-index: 50;
}
</style>

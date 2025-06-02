<template>
  <div class="h-screen flex flex-col text-gray-700 font-display bg-slate-100 gap-2">
    <!-- Header -->
    <header class="flex-shrink-0 bg-white shadow p-5">
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
    <div class="flex-shrink-0 max-w-6xl mx-auto w-full bg-white rounded-lg shadow-sm p-1">
      <div class="flex items-center justify-between w-full">
        <NuxtLink
          v-for="link in navigationLinks"
          :key="link.to"
          :to="link.to"
          class="flex flex-col items-center flex-1 py-2 rounded-lg transition-all duration-200"
          :class="isLinkActive(link) ? 'text-primary bg-primary/10' : 'text-gray-600'"
        >
          <Icon :name="link.icon" size="24" />
          <span class="text-xs font-medium mt-1">{{ link.label }}</span>
        </NuxtLink>
      </div>
    </div>

    <!-- Main Content -->
    <div class="flex-1 overflow-hidden w-6xl mx-auto h-full">
      <ScrollArea class="h-full">
        <slot />
      </ScrollArea>
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

// Navigation Links Configuration
const navigationLinks = [
  {
    to: '/pulsa',
    icon: 'lucide:smartphone',
    label: 'Pulsa',
    matchType: 'exact', // exact match for route path
  },
  {
    to: '/paket-data',
    icon: 'lucide:wifi',
    label: 'Paket Data',
    matchType: 'exact',
  },
  {
    to: '/voucher-game',
    icon: 'lucide:gamepad',
    label: 'Voucher Game',
    matchType: 'startsWith', // starts with for nested routes
  },
  {
    to: '/tagihan',
    icon: 'heroicons:bolt-solid',
    label: 'Tagihan',
    matchType: 'exact',
  },
  {
    to: '/entertainment',
    icon: 'lucide:music',
    label: 'Entertainment',
    matchType: 'exact',
  },
  {
    to: '/lainnya',
    icon: 'lucide:more-horizontal',
    label: 'Lainnya',
    matchType: 'exact',
  },
];

// Function to check if link is active
const isLinkActive = (link) => {
  const route = useRoute();
  if (link.matchType === 'startsWith') {
    return route.path.startsWith(link.to);
  }
  return route.path === link.to;
};

// Provide selectedProduct to child components
provide('selectedProduct', selectedProduct);
</script>

<style scoped>
.fixed {
  position: fixed;
  z-index: 50;
}
</style>

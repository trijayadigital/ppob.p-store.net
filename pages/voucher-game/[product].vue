<script setup>
import { ref, computed } from 'vue';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { Button } from '@/components/ui/button';
import ProductSelector from '@/components/ProductSelector.vue';
import data from '@/data.json';

// Menggunakan layout products
definePageMeta({
  layout: 'products',
});

const route = useRoute();
const router = useRouter();

const selectedVariant = ref('');

// Form data
const formData = ref({
  userId: '',
  zoneId: '',
  email: '',
});

// Data dari JSON file
const games = ref(data.games);

// Computed untuk mendapatkan game berdasarkan parameter route
const selectedGameData = computed(() => {
  return games.value.find((game) => game.code === route.params.product);
});

// Computed untuk memformat variants data untuk ProductSelector
const firstTopUpVariants = computed(() => {
  if (!selectedGameData.value) return [];
  return selectedGameData.value.variants
    .filter((v) => v.name.includes('First Top Up'))
    .map((variant) => ({
      product_id: variant.code,
      product_name: variant.name,
      price_personal: variant.price.value,
      desc: `${variant.duration === 0 ? 'Instan' : `${variant.duration} hari`} • ${variant.region}`,
    }));
});

const specialItemsVariants = computed(() => {
  if (!selectedGameData.value) return [];
  return selectedGameData.value.variants
    .filter((v) => v.name.includes('Pass'))
    .map((variant) => ({
      product_id: variant.code,
      product_name: variant.name,
      price_personal: variant.price.value,
      desc: `${variant.duration === 0 ? 'Instan' : `${variant.duration} hari`} • ${variant.region}`,
    }));
});

const regularVariants = computed(() => {
  if (!selectedGameData.value) return [];
  return selectedGameData.value.variants
    .filter((v) => !v.name.includes('First Top Up') && !v.name.includes('Pass'))
    .map((variant) => ({
      product_id: variant.code,
      product_name: variant.name,
      price_personal: variant.price.value,
      desc: `${variant.duration === 0 ? 'Instan' : `${variant.duration} hari`} • ${variant.region}`,
    }));
});

// Inject selectedProduct dari layout untuk footer
const layoutSelectedProduct = inject('selectedProduct');

const selectVariant = (variantCode) => {
  selectedVariant.value = variantCode;

  // Update selected product untuk footer
  const variant = selectedGameData.value?.variants.find((v) => v.code === variantCode);
  if (layoutSelectedProduct && variant) {
    layoutSelectedProduct.value = {
      price_personal: variant.price.value,
    };
  }
};

const goBack = () => {
  router.push('/voucher-game');
};

// Redirect jika game tidak ditemukan
if (!selectedGameData.value) {
  throw createError({
    statusCode: 404,
    statusMessage: 'Game tidak ditemukan',
  });
}
</script>

<template>
  <div class="flex items-center gap-2 my-4 ml-4">
    <nav aria-label="Breadcrumb">
      <ol class="flex items-center space-x-2 text-sm text-gray-500">
        <li>
          <NuxtLink to="/" class="hover:text-gray-700 transition-colors"> Home </NuxtLink>
        </li>
        <li>
          <Icon name="lucide:chevron-right" size="14" class="text-gray-400" />
        </li>
        <li>
          <NuxtLink to="/voucher-game" class="hover:text-gray-700 transition-colors"> Voucher Game </NuxtLink>
        </li>
        <li>
          <Icon name="lucide:chevron-right" size="14" class="text-gray-400" />
        </li>
        <li class="text-gray-700 font-medium">
          {{ selectedGameData.name }}
        </li>
      </ol>
    </nav>
  </div>
  <Card>
    <CardHeader>
      <div class="flex flex-col items-start gap-4">
        <div class="flex gap-6">
          <NuxtImg :src="selectedGameData.image" :alt="selectedGameData.name" class="size-48 rounded-lg object-cover" />
          <div class="flex flex-col gap-1">
            <div class="text-lg font-bold">{{ selectedGameData.name }}</div>
            <div class="text-sm text-gray-500">{{ selectedGameData.developer }}</div>
            <div class="text-xs text-gray-600 space-y-1">
              <div class="font-medium text-gray-700 mb-2">Cara Top Up:</div>
              <ol class="list-decimal list-inside space-y-1 text-xs">
                <li v-for="(step, index) in selectedGameData.how_to" :key="index" class="text-gray-600">
                  {{ step }}
                </li>
              </ol>
            </div>
          </div>
        </div>
      </div>
    </CardHeader>

    <CardContent class="space-y-6">
      <!-- Varian Voucher -->
      <div class="space-y-4">
        <h4 class="text-lg font-semibold text-gray-900">Pilih Nominal</h4>

        <!-- First Top Up Section -->
        <div v-if="firstTopUpVariants.length > 0" class="space-y-3">
          <div class="flex items-center gap-2">
            <span class="text-red-500">🔥</span>
            <h5 class="font-semibold text-gray-800">First Top Up (Double Diamonds)</h5>
          </div>
          <ProductSelector :products="firstTopUpVariants" v-model="selectedVariant" :columns="4" :show-description="true" />
        </div>

        <!-- Special Items Section -->
        <div v-if="specialItemsVariants.length > 0" class="space-y-3">
          <div class="flex items-center gap-2">
            <span class="text-orange-500">🔥</span>
            <h5 class="font-semibold text-gray-800">Special Items</h5>
          </div>
          <ProductSelector :products="specialItemsVariants" v-model="selectedVariant" :columns="4" :show-description="true" />
        </div>

        <!-- Regular Top Up Section -->
        <div v-if="regularVariants.length > 0" class="space-y-3">
          <div class="flex items-center gap-2">
            <span class="text-yellow-500">⭐</span>
            <h5 class="font-semibold text-gray-800">Top Up Regular</h5>
          </div>
          <ProductSelector :products="regularVariants" v-model="selectedVariant" :columns="4" :show-description="true" />
        </div>
      </div>

      <!-- Konfirmasi Pilihan -->
      <div v-if="selectedVariant" class="mt-6 p-4 bg-green-50 border border-green-200 rounded-lg">
        <p class="text-green-800 font-medium">✓ Varian terpilih: {{ selectedGameData.variants.find((v) => v.code === selectedVariant)?.name }}</p>
        <p class="text-green-600 text-sm mt-1">
          Harga: Rp {{ selectedGameData.variants.find((v) => v.code === selectedVariant)?.price.value.toLocaleString('id-ID') }}
        </p>
      </div>

      <!-- Form Input Detail Akun -->
      <div v-if="selectedVariant" class="mt-8 space-y-6">
        <!-- Step 4: Detail Akun -->
        <Card>
          <CardHeader>
            <div class="flex items-center gap-3">
              <div class="w-8 h-8 bg-blue-500 text-white rounded-full flex items-center justify-center font-bold text-sm">4</div>
              <div>
                <CardTitle class="text-lg">Masukkan detail akun</CardTitle>
                <div class="flex items-center gap-2 mt-1">
                  <span class="w-2 h-2 bg-blue-500 rounded-full"></span>
                  <span class="text-sm text-blue-600 font-medium">Panduan</span>
                </div>
              </div>
            </div>
          </CardHeader>
          <CardContent class="space-y-4">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <!-- Dynamic inputs based on game configuration -->
              <div v-for="input in selectedGameData.inputs" :key="input.name" class="space-y-2">
                <Label :for="input.name">{{ input.title }}</Label>
                <Input :id="input.name" v-model="formData[input.name]" :type="input.type" :placeholder="`Masukkan ${input.title}`" class="w-full" />
              </div>
            </div>

            <!-- Instruksi -->
            <div class="p-3 bg-blue-50 border border-blue-200 rounded-lg">
              <p class="text-sm text-blue-800">
                {{ selectedGameData.instructions }}
                <button class="text-blue-600 underline ml-1">Selengkapnya</button>
              </p>
            </div>
          </CardContent>
        </Card>

        <!-- Step 5: Info Kontak -->
        <Card>
          <CardHeader>
            <div class="flex items-center gap-3">
              <div class="w-8 h-8 bg-pink-500 text-white rounded-full flex items-center justify-center font-bold text-sm">5</div>
              <CardTitle class="text-lg">Masukkan info kontak</CardTitle>
            </div>
          </CardHeader>
          <CardContent class="space-y-4">
            <div class="space-y-2">
              <Label for="email">Email (opsional)</Label>
              <Input id="email" v-model="formData.email" type="email" placeholder="Masukkan email" class="w-full" />
              <p class="text-sm text-gray-600">Kamu bisa isi jika mau terima bukti transaksi</p>
            </div>
          </CardContent>
        </Card>
      </div>
    </CardContent>
  </Card>
</template>

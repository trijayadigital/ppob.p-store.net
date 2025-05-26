<script setup>
import { ref, computed } from 'vue';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import data from '@/data.json';

// Menggunakan layout products
definePageMeta({
  layout: 'products',
});

const selectedService = ref('');
const selectedVariant = ref('');

// Form data
const formData = ref({});

// Data dari JSON file
const entertainments = ref(data.entertainments);

// Computed untuk mendapatkan service yang dipilih
const selectedServiceData = computed(() => {
  return entertainments.value.find((service) => service.id === selectedService.value);
});

const selectService = (serviceId) => {
  selectedService.value = serviceId;
  selectedVariant.value = ''; // Reset variant selection
  // Reset form data
  formData.value = {};
};

const selectVariant = (variantCode) => {
  selectedVariant.value = variantCode;
};
</script>

<template>
  <Card>
    <CardHeader>
      <CardTitle>Entertainment</CardTitle>
      <CardDescription>Pilih layanan hiburan favorit Anda</CardDescription>
    </CardHeader>
    <CardContent class="space-y-6">
      <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-4">
        <Card
          v-for="service in entertainments"
          :key="service.id"
          class="group cursor-pointer border-2 overflow-hidden transition-all duration-200 hover:shadow-lg hover:scale-105"
          :class="selectedService === service.id ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
          @click="selectService(service.id)"
        >
          <CardContent class="p-0">
            <div class="relative overflow-hidden">
              <NuxtImg
                :src="service.image"
                :alt="service.name"
                class="w-full h-48 object-cover transition-transform duration-300 group-hover:scale-110"
                loading="lazy"
              />
              <div class="absolute top-2 right-2">
                <span class="inline-block px-2 py-1 bg-black/70 text-white text-xs rounded-full backdrop-blur-sm">
                  {{ service.developer }}
                </span>
              </div>
            </div>
            <div class="p-4 space-y-2">
              <div class="text-center text-lg font-semibold text-gray-900">
                {{ service.name }}
              </div>
              <div class="text-center text-sm text-gray-600">
                {{ service.developer }}
              </div>
            </div>
          </CardContent>
        </Card>
      </div>

      <!-- Tampilan Varian Layanan -->
      <div v-if="selectedServiceData" class="mt-8 space-y-6">
        <div class="flex items-center gap-4">
          <div class="flex items-center gap-3">
            <NuxtImg :src="selectedServiceData.image" :alt="selectedServiceData.name" class="w-12 h-12 rounded-lg object-cover" />
            <div>
              <h3 class="text-xl font-bold text-gray-900">{{ selectedServiceData.name }}</h3>
              <p class="text-sm text-gray-600">{{ selectedServiceData.developer }}</p>
            </div>
          </div>
          <Badge class="bg-purple-500 text-white"> Entertainment </Badge>
        </div>

        <!-- Varian Layanan -->
        <div class="space-y-4">
          <h4 class="text-lg font-semibold text-gray-900">Pilih Paket</h4>

          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3">
            <Card
              v-for="variant in selectedServiceData.variants"
              :key="variant.code"
              class="cursor-pointer border-2 transition-all duration-200 hover:shadow-md"
              :class="selectedVariant === variant.code ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
              @click="selectVariant(variant.code)"
            >
              <CardContent class="p-4">
                <div class="flex items-center gap-3">
                  <div class="w-12 h-12 bg-purple-500 rounded-lg flex items-center justify-center">
                    <span class="text-white text-xl">🎬</span>
                  </div>
                  <div class="flex-1">
                    <h6 class="font-semibold text-sm text-gray-900">{{ variant.name }}</h6>
                    <p class="text-lg font-bold text-orange-600">Rp {{ variant.price.value.toLocaleString('id-ID') }}</p>
                  </div>
                  <button class="px-3 py-1 bg-gray-100 text-gray-700 text-xs rounded-full hover:bg-gray-200">INSTANT</button>
                </div>
              </CardContent>
            </Card>
          </div>
        </div>

        <!-- Konfirmasi Pilihan -->
        <div v-if="selectedVariant" class="mt-6 p-4 bg-green-50 border border-green-200 rounded-lg">
          <p class="text-green-800 font-medium">✓ Paket terpilih: {{ selectedServiceData.variants.find((v) => v.code === selectedVariant)?.name }}</p>
          <p class="text-green-600 text-sm mt-1">
            Harga: Rp {{ selectedServiceData.variants.find((v) => v.code === selectedVariant)?.price.value.toLocaleString('id-ID') }}
          </p>
        </div>

        <!-- Form Input Detail Akun -->
        <div v-if="selectedVariant" class="mt-8 space-y-6">
          <!-- Step 4: Detail Akun -->
          <Card>
            <CardHeader>
              <div class="flex items-center gap-3">
                <div class="w-8 h-8 bg-purple-500 text-white rounded-full flex items-center justify-center font-bold text-sm">4</div>
                <div>
                  <CardTitle class="text-lg">Masukkan detail akun</CardTitle>
                  <div class="flex items-center gap-2 mt-1">
                    <span class="w-2 h-2 bg-purple-500 rounded-full"></span>
                    <span class="text-sm text-purple-600 font-medium">Panduan</span>
                  </div>
                </div>
              </div>
            </CardHeader>
            <CardContent class="space-y-4">
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <!-- Dynamic inputs based on service configuration -->
                <div v-for="input in selectedServiceData.inputs" :key="input.name" class="space-y-2">
                  <Label :for="input.name">{{ input.title }}</Label>
                  <Input :id="input.name" v-model="formData[input.name]" :type="input.type" :placeholder="`Masukkan ${input.title}`" class="w-full" />
                </div>
              </div>

              <!-- Instruksi -->
              <div class="p-3 bg-purple-50 border border-purple-200 rounded-lg">
                <p class="text-sm text-purple-800">
                  {{ selectedServiceData.instructions }}
                  <button class="text-purple-600 underline ml-1">Selengkapnya</button>
                </p>
              </div>
            </CardContent>
          </Card>

          <!-- Tombol Lanjutkan -->
          <div class="flex justify-end">
            <button
              class="px-6 py-3 bg-purple-600 text-white rounded-lg font-semibold hover:bg-purple-700 transition-colors disabled:bg-gray-400 disabled:cursor-not-allowed"
              :disabled="!formData[selectedServiceData.inputs[0]?.name]"
            >
              Lanjutkan Pembayaran
            </button>
          </div>
        </div>
      </div>
    </CardContent>
  </Card>
</template>

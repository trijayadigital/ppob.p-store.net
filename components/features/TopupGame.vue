<script setup>
import { ref, computed } from 'vue';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import data from '@/data.json';

const emit = defineEmits(['update:selectedGame', 'update:selectedVariant']);

const selectedGame = ref('');
const selectedVariant = ref('');

// Form data
const formData = ref({
  userId: '',
  zoneId: '',
  email: '',
});

// Data dari JSON file
const games = ref(data.games);

// Computed untuk mendapatkan game yang dipilih
const selectedGameData = computed(() => {
  return games.value.find((game) => game.id === selectedGame.value);
});

const selectGame = (gameId) => {
  selectedGame.value = gameId;
  selectedVariant.value = ''; // Reset variant selection
  // Reset form data
  formData.value = {
    userId: '',
    zoneId: '',
    email: '',
  };
  emit('update:selectedGame', gameId);
};

const selectVariant = (variantCode) => {
  selectedVariant.value = variantCode;
  emit('update:selectedVariant', variantCode);
};
</script>

<template>
  <div class="space-y-6">
    <div>
      <h2 class="text-2xl font-bold text-gray-900 mb-2">Top Up Game Langsung</h2>
      <p class="text-gray-600">Pilih game favorit Anda untuk melakukan top up voucher</p>
    </div>

    <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-4">
      <Card
        v-for="game in games"
        :key="game.id"
        class="group cursor-pointer border-2 overflow-hidden transition-all duration-200 hover:shadow-lg hover:scale-105"
        :class="selectedGame === game.id ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
        @click="selectGame(game.id)"
      >
        <CardContent class="p-0">
          <div class="relative overflow-hidden">
            <NuxtImg
              :src="game.image"
              :alt="game.name"
              class="w-full h-48 object-cover transition-transform duration-300 group-hover:scale-110"
              loading="lazy"
            />
            <div class="absolute top-2 right-2">
              <span class="inline-block px-2 py-1 bg-black/70 text-white text-xs rounded-full backdrop-blur-sm">
                {{ game.category }}
              </span>
            </div>
          </div>
          <div class="p-4 space-y-2">
            <div class="text-center text-lg font-semibold text-gray-900">
              {{ game.name }}
            </div>
            <div class="text-center text-sm text-gray-600">
              {{ game.description }}
            </div>
          </div>
        </CardContent>
      </Card>
    </div>

    <!-- Tampilan Varian Voucher -->
    <div v-if="selectedGameData" class="mt-8 space-y-6">
      <div class="flex items-center gap-4">
        <div class="flex items-center gap-3">
          <NuxtImg :src="selectedGameData.image" :alt="selectedGameData.name" class="w-12 h-12 rounded-lg object-cover" />
          <div>
            <h3 class="text-xl font-bold text-gray-900">{{ selectedGameData.name }}</h3>
            <p class="text-sm text-gray-600">{{ selectedGameData.description }}</p>
          </div>
        </div>
        <Badge :class="selectedGameData.color" class="text-white">
          {{ selectedGameData.category }}
        </Badge>
      </div>

      <!-- Varian Voucher -->
      <div class="space-y-4">
        <h4 class="text-lg font-semibold text-gray-900">Pilih Nominal</h4>

        <!-- First Top Up Section -->
        <div v-if="selectedGameData.variants.some((v) => v.name.includes('First Top Up'))" class="space-y-3">
          <div class="flex items-center gap-2">
            <span class="text-red-500">🔥</span>
            <h5 class="font-semibold text-gray-800">First Top Up (Double Diamonds)</h5>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3">
            <Card
              v-for="variant in selectedGameData.variants.filter((v) => v.name.includes('First Top Up'))"
              :key="variant.code"
              class="cursor-pointer border-2 transition-all duration-200 hover:shadow-md"
              :class="selectedVariant === variant.code ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
              @click="selectVariant(variant.code)"
            >
              <CardContent class="p-4">
                <div class="flex items-center gap-3">
                  <div class="w-12 h-12 bg-blue-500 rounded-lg flex items-center justify-center">
                    <span class="text-white text-xl">💎</span>
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

        <!-- Special Items Section -->
        <div v-if="selectedGameData.variants.some((v) => v.name.includes('Pass'))" class="space-y-3">
          <div class="flex items-center gap-2">
            <span class="text-orange-500">🔥</span>
            <h5 class="font-semibold text-gray-800">Special Items</h5>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
            <Card
              v-for="variant in selectedGameData.variants.filter((v) => v.name.includes('Pass'))"
              :key="variant.code"
              class="cursor-pointer border-2 transition-all duration-200 hover:shadow-md"
              :class="selectedVariant === variant.code ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
              @click="selectVariant(variant.code)"
            >
              <CardContent class="p-4">
                <div class="flex items-center gap-3">
                  <div class="w-12 h-12 bg-purple-500 rounded-lg flex items-center justify-center">
                    <span class="text-white text-xl">🎫</span>
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

        <!-- Regular Top Up Section -->
        <div v-if="selectedGameData.variants.some((v) => !v.name.includes('First Top Up') && !v.name.includes('Pass'))" class="space-y-3">
          <div class="flex items-center gap-2">
            <span class="text-yellow-500">⭐</span>
            <h5 class="font-semibold text-gray-800">
              Top Up
              {{
                selectedGameData.name.includes('Mobile Legends')
                  ? 'Diamonds'
                  : selectedGameData.name.includes('Free Fire')
                  ? 'Diamonds'
                  : selectedGameData.name.includes('PUBG')
                  ? 'UC'
                  : selectedGameData.name.includes('Genshin')
                  ? 'Genesis Crystals'
                  : selectedGameData.name.includes('Valorant')
                  ? 'VP'
                  : selectedGameData.name.includes('Call of Duty')
                  ? 'CP'
                  : selectedGameData.name.includes('Arena')
                  ? 'Vouchers'
                  : selectedGameData.name.includes('Honkai')
                  ? 'Crystals'
                  : 'Gems'
              }}
            </h5>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3">
            <Card
              v-for="variant in selectedGameData.variants.filter((v) => !v.name.includes('First Top Up') && !v.name.includes('Pass'))"
              :key="variant.code"
              class="cursor-pointer border-2 transition-all duration-200 hover:shadow-md"
              :class="selectedVariant === variant.code ? 'border-primary bg-primary/5' : 'border-gray-200 hover:border-primary/50'"
              @click="selectVariant(variant.code)"
            >
              <CardContent class="p-4">
                <div class="flex items-center gap-3">
                  <div class="w-12 h-12 bg-blue-500 rounded-lg flex items-center justify-center">
                    <span class="text-white text-xl">💎</span>
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
              <!-- User ID -->
              <div class="space-y-2">
                <Label for="userId">{{ selectedGameData.userIdLabel }}</Label>
                <Input id="userId" v-model="formData.userId" :placeholder="selectedGameData.userIdPlaceholder" class="w-full" />
              </div>

              <!-- Zone ID (jika diperlukan) -->
              <div v-if="selectedGameData.requiresZoneId" class="space-y-2">
                <Label for="zoneId">{{ selectedGameData.zoneIdLabel }}</Label>
                <Input id="zoneId" v-model="formData.zoneId" :placeholder="selectedGameData.zoneIdPlaceholder" class="w-full" />
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

        <!-- Tombol Lanjutkan -->
        <div class="flex justify-end">
          <button
            class="px-6 py-3 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 transition-colors disabled:bg-gray-400 disabled:cursor-not-allowed"
            :disabled="!formData.userId || (selectedGameData.requiresZoneId && !formData.zoneId)"
          >
            Lanjutkan Pembayaran
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
